### merge 语句的使用小技巧

***merge***语句在使用中，可以i通过关联查询方式，来将部分业务逻辑写入其中，可以在一定情况下避免高并发带来的问题。

例如：

（背景：光大银行 - 数字名片 - 众测体验官需求）

在进行体验官注册的逻辑时，分为员工和客户两种类型的体验官；

通过 ***子查询条件*** 以及 ***更新语句中的条件***，可以最大情况的避免数据库的误操作，导致数据错误。

```sql
/**客户体验官注册**/
merge into FOCKTEST_OFFICER_INFO T1
                using (select #{phone} as PHONE
                       from dual
                       --确保unionId未使用（确保该三方用户不是客户体验官）
                       where not exists(
                               select 1
                               from FOCKTEST_OFFICER_INFO
                               where UNION_ID = #{unionId}
                                 and UNION_ID_TYPE = '1')
                        --确保phone未使用（未注册为员工体验官，未注册为客户体验官）
                         and not exists(select 1
                                        from FOCKTEST_OFFICER_INFO
                                        where (UNION_ID = 'guangdajia'
                                                and UNION_ID_TYPE = '0'
                                                and PHONE = #{phone})
                                           or (T1.UNION_ID is not null
                                                and T1.UNION_ID_TYPE = '1'
                                                and T1.PHONE = #{phone}))
                        ) T2
                on (t1.PHONE = t2.PHONE)
                when matched then
                        update
                        set t1.UNION_ID           = #{unionId},
                            t1.UPDATE_TIME        = systimestamp,
                            t1.IS_STAFF           = '0',
                            t1.OPER_KEY           = #{managerId},
                            t1.OPER_NO            = (select t.LOGNAME
                                                     from EC_ORG_PERSON t
                                                     where t.OPER_KEY = #{managerId}),
                            t1.NICKNAME           = #{nickName},
                            t1.FLAG               = '1',
                            t1.UNION_ID_TYPE      = '1',
                            t1.BRANCH_FIRST_ID    = #{firstId},
                            t1.BRANCH_FIRST_NAME  = #{firstName},
                            t1.BRANCH_SECOND_ID   = #{secondId},
                            t1.BRANCH_SECOND_NAME = #{secondName},
                            t1.SUB_ID             = #{subId},
                            t1.SUB_NAME           = #{subName}
                        --确保可以升级的用户类型（白名单用户）
                        where t1.UNION_ID is null
                when not matched then
                        insert (ID, NICKNAME, BRANCH_FIRST_ID, BRANCH_FIRST_NAME, BRANCH_SECOND_ID,
                                BRANCH_SECOND_NAME, SUB_ID, SUB_NAME, IS_STAFF, PHONE, ADD_TIME, UPDATE_TIME,
                                OPER_NO, DEFAULT_LEVEL, RECRUIT_TIME, FLAG, DATA_SOURCE, UNION_ID,
                                UNION_ID_TYPE, OPER_KEY)
                        values (#{id}, #{nickName}, #{firstId}, #{firstName}, #{secondId}, #{secondName}, #{subId},
                                #{subName}, '0',
                                #{phone}, systimestamp, systimestamp, (select t.LOGNAME
                                                                       from EC_ORG_PERSON t
                                                                       where t.OPER_KEY = #{managerId}), '0',
                                to_char(sysdate, 'yyyy-MM-DD'), '1', 'EBDC',
                                #{unionId}, '1', #{managerId})
```



```sql
/**员工体验官注册**/
merge into FOCKTEST_OFFICER_INFO T1
                using (select #{phone} as PHONE
                       from dual
                       --确保operKey未使用（该客户经理不是员工体验官）
                       where not exists(
                               select 1
                               from FOCKTEST_OFFICER_INFO
                               where UNION_ID = 'guangdajia'
                                 and UNION_ID_TYPE = '0'
                                 and OPER_KEY = #{managerId})
                        --确保phone未使用（未注册为员工体验官）
                         and not exists(select 1
                                        from FOCKTEST_OFFICER_INFO
                                        where UNION_ID = 'guangdajia'
                                          and UNION_ID_TYPE = '0'
                                          and PHONE = #{phone})
                        ) T2
                on (t1.PHONE = t2.PHONE)
                when matched then
                        update
                        set t1.UNION_ID           = #{unionId},
                            t1.UPDATE_TIME        = systimestamp,
                            t1.IS_STAFF           = '1',
                            t1.OPER_KEY           = #{managerId},
                            t1.OPER_NO            = (select t.LOGNAME
                                                     from EC_ORG_PERSON t
                                                     where t.OPER_KEY = #{managerId}),
                            t1.NICKNAME           = #{name},
                            t1.FLAG               = '1',
                            t1.UNION_ID_TYPE      = '0',
                            t1.BRANCH_FIRST_ID    = #{firstId},
                            t1.BRANCH_FIRST_NAME  = #{firstName},
                            t1.BRANCH_SECOND_ID   = #{secondId},
                            t1.BRANCH_SECOND_NAME = #{secondName},
                            t1.SUB_ID             = #{subId},
                            t1.SUB_NAME           = #{subName}
                        --确保可以升级的用户类型（白名单用户，客户体验官）
                        where t1.UNION_ID is null
                           or (t1.UNION_ID_TYPE = '1' and t1.UNION_ID is not null)
                when not matched then
                        insert (ID, NICKNAME, BRANCH_FIRST_ID, BRANCH_FIRST_NAME, BRANCH_SECOND_ID,
                                BRANCH_SECOND_NAME, SUB_ID, SUB_NAME, IS_STAFF, PHONE, ADD_TIME, UPDATE_TIME,
                                OPER_NO, DEFAULT_LEVEL, RECRUIT_TIME, FLAG, DATA_SOURCE, UNION_ID,
                                UNION_ID_TYPE, OPER_KEY)
                        values (#{id}, #{name}, #{firstId}, #{firstName}, #{secondId}, #{secondName}, #{subId},
                                #{subName}, '1',
                                #{phone}, systimestamp, systimestamp, (select t.LOGNAME
                                                                       from EC_ORG_PERSON t
                                                                       where t.OPER_KEY = #{managerId}), '0',
                                to_char(sysdate, 'yyyy-MM-DD'), '1', 'EBDC',
                                #{unionId}, '0', #{managerId})
```

