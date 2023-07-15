# строке

BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));

&#x20;       String s = reader.readLine();

&#x20;       char\[] ch = s.toCharArray();

&#x20;       for(int i = 0; i < ch.length; i++){

&#x20;           // Если первый символ буква

&#x20;           if(i == 0 && Character.isLetter(ch\[i]))

&#x20;               ch\[i] = Character.toUpperCase(ch\[i]);

&#x20;           else if(i!=0&\&Character.**isSpaceChar(ch\[i-1])** && Character.isLetter(ch\[i]))

&#x20;               ch\[i] = Character.toUpperCase(ch\[i]);

&#x20;       }

&#x20;       s = String.copyValueOf(ch);

&#x20;       System.out.println(s);
