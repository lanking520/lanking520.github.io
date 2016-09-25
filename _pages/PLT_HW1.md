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
### Testing Result:
```
Lanking-3:Ocaml lanking$ ./wordcount < wordcount.mll
14 word
7 map
7 let
7 in
7 count
7 StringMap
5 my
4 tuple
4 token
4 list
4 fun
4 c
3 next
3 lexbuf
3 l
3 a
3 Word
3 List
3 EOF
2 wordlist
2 wordcounts
2 value
2 string
2 s
2 of
2 num
2 key
2 fold
2 b
2 add
1 z
1 with
1 type
1 then
1 stdin
1 sort
1 rule
1 rec
1 print
1 parse
1 n
1 module
1 mem
1 match
1 left
1 iter
1 int
1 if
1 from
1 find
1 eof
1 endline
1 empty
1 else
1 compare
1 channel
1 as
1 Z
1 String
1 Pervasives
1 Map
1 Make
1 Lexing
1 A
```
## Part 3: The calculator
### Thinking
Firstly we need to understand what the funcationality of each file.
### Main Code
#### ast.mli
```ocaml
type operator = Add | Sub | Mul | Div

type expr = 
    Binop of expr * operator * expr 
    | Lit of int
    | Seq of expr * expr
    | Asn of int * expr
    | Var of int
```
#### scanner.mll
```ocaml
{ open Parser}

rule token = 
  parse [' ' '\t' '\r' '\n'] {token lexbuf}
  | '+'                      {PLUS}
  | '-'                      {MINUS}
  | '*'                      {TIMES}
  | '/'                      {DIVIDE}
  | '='			     {EQUAL}
  | ','			     {COMMA}
  | ['0' - '9']+ as lit      {LITERAL(int_of_string lit)}
  | '$'['0' - '9'] as lit    {VARIABLE(int_of_char lit.[1] - 48)}
  | eof                      {EOF}
```
#### parser.mly
```ocaml
%{ open Ast %}

%token PLUS MINUS TIMES DIVIDE EQUAL COMMA EOF
%token <int> LITERAL
%token <int> VARIABLE
%left EQUAL COMMA
%left PLUS MINUS
%left TIMES DIVIDE
%start expr
%type <Ast.expr> expr

%%

expr:
    expr PLUS expr   		{Binop($1, Add, $3)}
  | expr MINUS expr  		{Binop($1, Sub, $3)}
  | expr TIMES expr  		{Binop($1, Mul, $3)}
  | expr DIVIDE expr 		{Binop($1, Div, $3)}
  | LITERAL          		{Lit($1)}
  | VARIABLE		 	{Var($1)}
  | VARIABLE EQUAL expr  	{Asn($1, $3)}
  | expr COMMA expr  		{Seq($1, $3)}
```
#### calc.ml
```ocaml
open Ast

let var_array = Array.make 10 0

let rec eval = function
  Lit(x) -> x
| Var(a) -> Array.get var_array a
| Seq(e1, e2) -> let v1 = eval e1 and v2 = eval e2 in v1 - v1 + v2
| Asn(e1, e2) -> Array.set var_array e1 (eval e2); eval e2
| Binop(e1, op, e2) ->
    let v1 = eval e1 and v2 = eval e2 in 
    match op with
      Add -> v1 + v2
    | Sub -> v1 - v2
    | Mul -> v1 * v2
    | Div -> v1 / v2

let _ = 
  let lexbuf = Lexing.from_channel stdin in 
  let expr = Parser.expr Scanner.token lexbuf  in
  let result = eval expr in
  print_endline (string_of_int result)
```
