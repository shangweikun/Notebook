```java

public Object setResultObjectValues(StatementScope statementScope, Object resultObject, Object[] values) {
   	...
    if (uniqueKeys != null && uniqueKeys.containsKey(ukey)) {
      ...
    } else if (ukey == null || uniqueKeys == null || !uniqueKeys.containsKey(ukey)) {
      // Unique key is NOT known, so create a new result object and then process additional
      // results.
      resultObject = dataExchange.setData(statementScope, this, resultObject, values);
      // Lazy init key set, only if we're grouped by something (i.e. ukey != null)
      ...
    } else {
      // Otherwise, we don't care about these results.
      resultObject = NO_VALUE;
    }
    ...
    return resultObject;
  }
```

```java
private void handleResults(StatementScope statementScope, ResultSet rs, int skipResults, int maxResults,
      RowHandlerCallback callback) throws SQLException {
    	
    ...

        // Get Results
        int resultsFetched = 0;
        while ((maxResults == NO_MAXIMUM_RESULTS || resultsFetched < maxResults) && rs.next()) {
          Object[] columnValues = resultMap.resolveSubMap(statementScope, rs).getResults(statementScope, rs);
          callback.handleResultObject(statementScope, columnValues, rs);
          resultsFetched++;
        }
      }
    } finally {
      statementScope.setResultSet(null);
    }

   ...
       
  }
```

