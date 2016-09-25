# COMS W4115 Homework Assignment 1

UNI: ql2282 Name: Qing Lan

## Part 1: The Oxford Function
### Thinking
There will be three different cases, set the example first and then implement the main function. 
The conbination of the string list could not be in a for or while loop, so implement it with recursion. The main method is "^"(join)
### Main code
```ocaml
let example1 = ["one"];; (*case 1*)
let example2 = ["one";"two"];; (*case2*)
let example3 = ["one";"two";"three";"four"];; (*case3*)

let rec oxford = function
	[] ->  ""
	| [a] ->  a (*return one if only one*)
	| [a; b] -> a^" and "^b (*return normal and if there are two*)
	| [a; b; c] -> a^", "^b^", and "^c (*return comma + and if there are three*)
	| hd::tl -> hd^", "^oxford tl;; (*Recursion*)

print_endline (oxford example1);;
print_endline (oxford example2);;
print_endline (oxford example3);;
```
### Testing Result:
```
Lanking-3:~ lanking$ ocaml /Users/lanking/Desktop/Ocaml/HW1.ml 
one
one and two
one, two, three, and four
```

## Part 2: Word Frequency Counter
### Thinking
with a empty string list provided, the map should be implemented to count all words. The general idea is to create a recursion function to loop all words. Everytime pass in the remaining string list and the map. Then, exporting the map to tuple list using map.fold to loop. Finally sort it and print them all.
### Main code
```ocaml
{ type token = EOF | Word of string }
rule token = parse
| eof { EOF }
| ['a'-'z' 'A'-'Z']+ as word { Word(word) }
| _ { token lexbuf }
{
module StringMap = Map.Make(String);;
let lexbuf = Lexing.from_channel stdin in
let wordlist =
let rec next l =
match token lexbuf with
EOF -> l
| Word(s) -> next (s :: l )
in next []
in
let word_count_map = List.fold_left(fun word_count_map my_word -> 
					if StringMap.mem my_word word_count_map
					then let num = StringMap.find my_word word_count_map in
							StringMap.add my_word (num+1) word_count_map
					else StringMap.add my_word 1 word_count_map) StringMap.empty wordlist in
let tuple_list = StringMap.fold(fun key value tuple_list-> (value, key) :: tuple_list) word_count_map [] in
let wordcounts = List.sort(fun (c1,_) (c2,_) -> Pervasives.compare c2 c1) tuple_list in
List.iter (fun (a,b) -> print_endline ((string_of_int a)^" "^b^"\n")) wordcounts
}
```
