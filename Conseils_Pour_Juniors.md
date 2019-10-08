Vous allez avoir, forcèment un boucle qui va itérer des milliers, des millions voire pour les derniers tests des milliards
 de fois, donc autant vous dire que le contenu de la boucle doit être le plus léger et rapide possible.
 
La première astuce est de laisser Ruby résoudre tout seul le puzzle, en "compilant" avec [`Kernel#eval`](https://ruby-doc.org/core-2.6.3/Kernel.html#method-i-eval) 
le puzzle de façon à cre qu'il ne vous reste plus qu'à lui envoyer les chiffres et voir quel combinaisons de chiffres 
retourne `true`  

Pour vous aider, voici un extrait d'un excellent livre sur Ruby, **Metaprogramming Ruby 2** de Paolo Perrotta chez Pragmatic 
Bookshelf

>###Kernel#eval  
>    
>    *Where you learn that, when it comes right down to it, code is just text.*
>    
>    You already learned about instance_eval and class_eval (in ​instance_eval()​, and ​class_eval()​, respectively). Now you can 
>    get acquainted with the third member of the *eval family—a Kernel Method (Kernel Method) that’s simply named eval. Kernel#eval
>    is the most straightforward of the three *eval methods. Instead of a block, it takes a string that contains Ruby, String 
>    of Code for short. Kernel#eval executes the code in the string and returns the result:  
>    
>######simple_eval.rb
>    ```ruby​
>    array = [10, 20]
>    ​ 	
>    element = 30
>    ​ 	
>    eval(​"array << element"​)  ​# => [10, 20, 30]​
>    ```
>    Executing a literal string of Ruby code is a pretty pointless exercise, but the power of eval becomes apparent when you 
>    compute your Strings of Code on the fly. Here’s an example.  
>    
>####The REST Client Example
>    REST Client (installed with gem install rest-client) is a simple HTTP client library. It includes an interpreter where 
>    you can issue regular Ruby commands together with HTTP methods such as get:
>    ```ruby
>    ​#=>	restclient http://www.twitter.com​
>    ​
>     html_first_chars = get(​"/"​)[0..14]
>    ​	
>    #=> ​"<!DOCTYPE html>"​
>    ```
>    
>    If you look in the gem’s source, you will see that get and the three other basic HTTP methods are defined on the Resource 
>    class:  
>    
>######gems/rest-client-1.6.7/lib/restclient/resource.rb    
>    ```ruby
>    ​module​ RestClient  
>    ​
>      ​class​ Resource
>    ​
>        ​def​ get(additional_headers={}, &block) ​# ...​
>    ​
>        ​def​ post(payload, additional_headers={}, &block) ​# ...​
>    ​
>        ​def​ put(payload, additional_headers={}, &block) ​# ...    ​
>        ​def​ delete(additional_headers={}, &block) ​# ...​
>        # ...
>      end
>    end
>    ```  
>    
>    To make get and its siblings available in the interpreter, REST Client defines four top-level methods that delegate to 
>    by the r method):
>    
>    ```ruby
>    ​def​  get(path, *args, &b)	
>      r[path].get(*args, &b)
>    ​end​
>    ```
>    
>    You might expect to find this definition of get in the source code, together with similar definitions for put, post, and 
>    delete. However, here comes a twist. Instead of defining the four methods separately, REST Client defines all of them in 
>    one shot by creating and evaluating four Strings of Code (String of Code) in a loop:
>    
>######gems/rest-client-1.6.7/bin/restclient​ 
>    ```ruby
>    POSSIBLE_VERBS = [​'get'​, ​'put'​, ​'post'​, ​'delete'​]
>    
>    ​
>    POSSIBLE_VERBS.each ​do​ |m|
>    ​
>      eval ​<<-end_eval​
>    ​    def  ​#{m}​(path, *args, &b)​
>    ​        r[path].​#{m}​(*args, &b)​
>    ​    end​
>    ​  end_eval​
>    ​ 	
>    ​end​
>    ```
>    
>    The code above uses an exotic syntax known as a here document, or heredoc for short. What you’re seeing after the eval 
>    is just a regular Ruby string, although it’s not delimited by the usual quotes. Instead, it starts with a <<- sequence 
>    followed by an arbitrary termination sequence—in this case, end_eval. The string ends on the first line that contains 
>    only the termination sequence, so this particular string spans the lines from the def to the first end included. The code 
>    uses regular string substitution to generate and eval four Strings of Code, one each for the definitions of get, put, post, 
>    and delete.  
>    
>    Most Strings of Code feature some kind of string substitution, as in the example above. For an alternate way to use eval, 
>    you can evaluate arbitrary Strings of Code from an external source, effectively building your own simple Ruby interpreter.

Donc avec `Kernel#eval` vous allez transformer un puzzle, par ex `A + B == C` en une méthode ou un `proc` du genre:  
```ruby
def solve?(a, b, c)
  a + b == c
end
```
Evidemment, vous n'allez pas coder en dur cette méthode ou ce proc pour chaque test, c'est à vous de créer 
une chaîne (ou un heredoc), comme dans l'exemple cité ci-dessus, et dans votre boucle, il vous suffira d'envoyer 
les valeurs numériques à tester avec [`Method#call`](https://ruby-doc.org/core-2.6.3/Method.html#method-i-call)
A vous de gérer les dizaines (ex `AB`), centaines, milliers etc...