# HashMap

public static void removeTheFirstNameDuplicates(HashMap\<String, String> map) {

&#x20;     &#x20;

&#x20;       ArrayList\<String> lst = new ArrayList\<String>(map.values());

&#x20;       int count;

&#x20;       for (String str : lst) {

&#x20;           count = 0;

&#x20;           for (String str2 : lst) {

&#x20;               if (str.equals(str2))

&#x20;                   count++;

&#x20;               if (count==2) removeItemFromMapByValue (map, str);

&#x20;           }

&#x20;       }

&#x20;   }

public static void removeItemFromMapByValue(HashMap\<String, String> map, String value) {

&#x20;       HashMap\<String, String> copy = new HashMap\<String, String>(map);

&#x20;       for (Map.Entry\<String, String> pair : copy.entrySet()) {

&#x20;           if (pair.getValue().equals(value))

&#x20;               map.remove(pair.getKey());

&#x20;       }

&#x20;   }
