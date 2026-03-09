# 15Practical

import java.nio.file.Files;
import java.util.*;
import java.io.*;

public class  anagrams{
    public static String signature(String Word) {

        ArrayList<Character> word = new ArrayList<>();
        for (int i = 0; i < word.size(); i++) {
            word.add(Word.charAt(i));
        }

        Collections.sort(word);
        StringBuilder result = new StringBuilder();

        for (char c : word) {
            result.append(c);
        }

        return result.toString();
    }
public static void main(String[] args){
        if(args.length==2){
            String inputfile=args[0];
            try {
                BufferedReader f=new BufferedReader(new FileReader(inputfile));
                System.out.println("Data file: "+ inputfile);

                Map<String,Integer>D=new HashMap<>();

                int linenumbers=0;

                 String line=f.readLine();
                 int lines=0;
                 while(line!=null){
                     linenumbers+=1;
                     String[] words=line.split("\\s+");

                     for(String w:words){
                         String W=w.replaceAll("[^a-zA-Z']","").toLowerCase();
                         if(w.isEmpty()) continue;
                         if(D.containsKey(w)){
                             D.put(w,D.get(w)+1);
                         }
                         else{
                             D.put(w,1);
                             lines++;
                         }
                     }
                 }
                f.close();
                 Map<String,ArrayList<String>>A=new HashMap<>();
                 for(String w: D.keySet()){
                     String a=signature(w);
                     if(!A.containsKey(a)){
                         A.put(a,new ArrayList<>());
                     }
                     else{
                         A.get(a).add(w);
                     }
                     BufferedWriter F=new BufferedWriter(new FileWriter("anagrams")) ;
                     for(String key:A.keySet()){
                       if(A.size()>1){
                           String anagramlist=null;
                           for(String word:A.keySet()){
                               if(anagramlist.isEmpty()) {
                                         anagramlist = word;
                               }
                               else{
                                   anagramlist+=""+word;
                               }
                           }
                           System.out.println((anagramlist + "\\\\\n")+f);
                           for(int repeat=0;repeat<A.get(key).size()-1;repeat++){
                               int space=anagramlist.indexOf("");

                               anagramlist = anagramlist.substring(space + 1) + " " + anagramlist.substring(0, space);
                               F.write(anagramlist+"\\\\\n");


                           }
                           F.close();

                           List<String>aa= Files.readAllLines("anagrams.tex");
                           Collections.sort(aa);

                           File latexDir=new File("latex");
                           if(!latexDir.exists()){
                               latexDir.mkdir();
                           }

                           BufferedWriter asftex=new BufferedWriter(new FileWriter("latex/theAnagrams.tex"));
                           char letter='X';
                           for(String lemma:aa){
                               int initial=lemma.charAt(0);

                               if(Character.toLowerCase(initial) != Character.toLowerCase(letter)){
                                   letter= initial;
                                   asftex.write("\n\\vspace{14pt}\n\\noindent\\textbf{\\Large %s}" +Character.toUpperCase(initial)+"}\\\\*[+12pt]\n");
                           }
                               asftex.write((lemma));



                       }
                           asftex.close();
                     }
                 }

            }

            }

            catch (IOException e){
                System.out.println("error file not found.");
            }
            catch (FileNotFoundException e){
            }
       }else{
             System.out.println("Usage: ./anagrams inputfile.\n You gave:\n %s"+  args[0]);
            }

        }

}

