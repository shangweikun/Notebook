### classless inter domain route

IP地址块222.125.80.128/26包含的可用主机数是多少，最小的地址是多少，最大的地址是多少？

[IP](https://www.baidu.com/s?wd=IP&from=1012015a&fenlei=mv6quAkxTZn0IZRqIHckPjm4nH00T1d9uAfvrjcvnH--mHnduHnd0ZwV5Hcvrjm3rH6sPfKWUMw85HfYnjn4nH6sgvPsT6KdThsqpZwYTjCEQLGCpyw9Uz4Bmy-bIi4WUvYETgN-TLwGUv3EnHRdnjcvP1fL)/26 是[CIDR](https://www.baidu.com/s?wd=CIDR&from=1012015a&fenlei=mv6quAkxTZn0IZRqIHckPjm4nH00T1d9uAfvrjcvnH--mHnduHnd0ZwV5Hcvrjm3rH6sPfKWUMw85HfYnjn4nH6sgvPsT6KdThsqpZwYTjCEQLGCpyw9Uz4Bmy-bIi4WUvYETgN-TLwGUv3EnHRdnjcvP1fL)的格式，全称是classless inter domain route 叫做无类域间路由，就是说32位[IP](https://www.baidu.com/s?wd=IP&from=1012015a&fenlei=mv6quAkxTZn0IZRqIHckPjm4nH00T1d9uAfvrjcvnH--mHnduHnd0ZwV5Hcvrjm3rH6sPfKWUMw85HfYnjn4nH6sgvPsT6KdThsqpZwYTjCEQLGCpyw9Uz4Bmy-bIi4WUvYETgN-TLwGUv3EnHRdnjcvP1fL)的前26位为网络号，后面的全部都可以分给主机：

222.125.80.128------------1101 1110.0111 1101.0101 0000.10（00 0000）

1101 1110.0111 1101.0101 0000.10 总共26位为网络号
括号里面是主机位，总共六位。 2^6=64
再除掉222.125.80.128即主机位全零地址，和222.125.80.191即[广播地址](https://www.baidu.com/s?wd=广播地址&from=1012015a&fenlei=mv6quAkxTZn0IZRqIHckPjm4nH00T1d9uAfvrjcvnH--mHnduHnd0ZwV5Hcvrjm3rH6sPfKWUMw85HfYnjn4nH6sgvPsT6KdThsqpZwYTjCEQLGCpyw9Uz4Bmy-bIi4WUvYETgN-TLwGUv3EnHRdnjcvP1fL)外
还剩64-2=62个主机地址。
其中最小地址主机位00 0001即222.125.80.129
最大地址主机位11 1110即222.125.80.190

