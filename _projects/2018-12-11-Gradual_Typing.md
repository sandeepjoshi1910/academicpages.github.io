---
title: 'Gradual Typing for Object Oriented Language'

permalink: /projects/gradool
tags:
  - Gradual Typing
  - Object Oriented Language
  - Type Consistency
  - Type checker
  - Interpreter
  - OCaml
---

### What is Gradual Typing?

[Gradual Typing](https://wphomes.soic.indiana.edu/jsiek/what-is-gradual-typing/) is a type system which integrates dynamic and static type system. Dynamic and Static type systems have their advantages and disadvantages. Dynamic type system allows rapid prototyping, is easy to learn and allows for more flexibility. On the other hand, Static typing ensures there are less bugs before hand, far faster than dynamically typed languages, since these are compiled, there is room for optimization. Also sound type systems ensure higher developer productivity. 

But when dealing with situations where we don't know the type of data (receiving data from a message queue), static type systems make it hard to work with and dynamic type systems allows us to simply cast them to the assumed data structure. Whereas static type systems have to go through series of steps for the received data to be useful.

Gradual typing aims to give programmer the option to use dynamic or static typing for any part of the code in a single language. Whcihever part of the code is statically typed is type checked and ensures there are no type errors in that part of the code.

### Implementation

Jeremy Siek introduces a concept called type consistency to facilitate the use of both typing systems. We introduce a new type called `Dynamic` which doesn't appear anywhere in the object inheritence hierarchy. This is different from the previous approaches which used upcasting to Object to enable untyped code to exist with statically typed code. But that creates problems because, once upcasted, downcasting is unsafe. One of the important aspects of gradual typing is statially typed part of the code should never have type errors during runtime.

#### Dynamic type and type consistency
A new type `Dynamic` is introduced
 and `~` refers to Consistent relation

**Type Consistency Rules**

* `Dynamic ~ any type`
* Any type is consistent with itself. `Int ~ Int`
* A type is not consistent with another type (unless its a super class type). `Int ~ Bool`
* A data structure with multiple types are consistent with another data structure if the corresponding types 
are consistent.
    Ex. `(Int,Bool,Dynamic) ~ (Int,Bool,Bool)`

#### Note:
* The [original paper on Gradual typing](http://scheme2006.cs.uchicago.edu/13-siek.pdf) and a later paper on [refined critera for gradual typing](http://homes.sice.indiana.edu/mvitouse/papers/snapl15.pdf) are specified for functional language. I am still trying to figure out the exact rules for type consistency in case of object oriented language.

* I have not implemented type inference and assumed a variable maps to a reference containing an object. Using type inference while type checking needs to be done.

## Code for the Interpreter and Gradual Type Checker

``` Swift

open List

type ident = string
type ref = int
type value = IntV of int | BoolV of bool | RefV of ref 

type typ = Tint | TBool | Dynamic | Tclass of ident

type exp = Int of int | Bool of bool | Add of exp * exp | Sub of exp * exp | Mul of exp * exp
            | Div of exp * exp | And of exp * exp | Or of exp * exp | Eq of exp * exp | Var of ident | GetField of exp * ident

type cmd = Assign of ident * exp | Seq of cmd * cmd | IfC of exp * cmd * cmd | Invoke of ident * exp * ident * exp list | Return of exp
            | SetField of exp * ident * exp | New of ident * ident * exp list 

type decl = Decl of typ * ident | Dseq of decl * decl

type mdecl = MDecl of typ * ident * (typ * ident) list * cmd

type cdecl = Class of ident * ident * (typ * ident) list * mdecl list

type class_table = ident -> cdecl option

type obj  = Obj of ident * (ident * value) list

let empty_state = fun x -> None

type state = ident -> value option

type tycon = ident -> typ option

let update s x v = fun y -> if y = x then Some v else s y

type env = ident -> value option

type store = (ref -> obj option) * ref

let empty_store : store = (empty_state, 0)
let store_lookup (s : store) = fst s
let next_ref (s : store) = snd s
let update_store (s : store) (r : ref) (o : obj) : store =
  (update (store_lookup s) r o, next_ref s)


let ctd c = if c = "Device" then Some( Class ("Device","Object", [(Tint, "length");(Tint, "width");(Tint, "height");(Tint, "weight")],
                                    [MDecl (Tint, "volume",[], 
                                             Return( Mul(GetField(Var "this","length"),Mul(GetField(Var "this","width"),GetField(Var "this","height")))));
                                    MDecl (Tint, "area",[],
                                             Return (Mul(GetField(Var "this","length"),GetField(Var "this","width"))))])) 
                            
                            else if c = "Laptop" then Some( Class("Laptop","Device", 
                                      [(Tint,"ScreenSize");(Tint,"resolution");(TBool,"hasRetina");(Tint, "RamSize");(Tint,"HDDSize")],
                                      [MDecl(Tint,"price",[],
                                        Return(Sub(Mul(Add(GetField(Var "this","RamSize"),GetField(Var "this","HDDSize")),Int 3),Mul(GetField(Var "this","weight"),Int 2))))]))

                            else None

let ctdy c = if c = "Device" then Some( Class ("Device","Object", [(Dynamic, "length");(Dynamic, "width");(Dynamic, "height");(Dynamic, "weight")],
                            [MDecl (Dynamic, "volume",[], 
                                     Return( Mul(GetField(Var "this","length"),Mul(GetField(Var "this","width"),GetField(Var "this","height")))));
                            MDecl (Dynamic, "area",[],
                                     Return (Mul(GetField(Var "this","length"),GetField(Var "this","width"))))])) 
                    
                    else if c = "Laptop" then Some( Class("Laptop","Device", 
                              [(Dynamic,"ScreenSize");(Dynamic,"resolution");(Dynamic,"hasRetina");(Dynamic, "RamSize");(Dynamic,"HDDSize")],
                              [MDecl(Dynamic,"price",[],
                                Return(Sub(Mul(Add(GetField(Var "this","RamSize"),GetField(Var "this","HDDSize")),Int 3),Mul(GetField(Var "this","weight"),Int 2))))]))

                    else None

let cth c = if c = "House" then Some ( Class("House","Object",[(Tint,"priceSqft")],
            [MDecl(Tint,"housePrice",
            [(TBool, "IsinDowntown");(Tint,"bedrooms");(Tint,"totalSqft")],
            IfC(Var "IsinDowntown", 
                Return(Mul(GetField(Var "this","priceSqft"),Add( Mul( Int 10, Var "bedrooms"),Var "totalSqft"))),
                Return(Mul(GetField(Var "this","priceSqft"),Add( Mul( Int 5, Var "bedrooms"),Var "totalSqft"))) ) )]))
    else None
    
let cthdy c = if c = "House" then Some ( Class("House","Object",[(Dynamic,"priceSqft")],
            [MDecl(Dynamic,"housePrice",
            [(TBool, "IsinDowntown");(Dynamic,"bedrooms");(Dynamic,"totalSqft")],
            IfC(Var "IsinDowntown", 
                Return(Mul(GetField(Var "this","priceSqft"),Add( Mul( Int 10, Var "bedrooms"),Var "totalSqft"))),
                Return(Mul(GetField(Var "this","priceSqft"),Add( Mul( Int 5, Var "bedrooms"),Var "totalSqft"))) ) )]))
else None    

let rec get_params (ids: (typ * ident) list) (lv: value list) : (ident * value) list = 
  (match ids, lv with
    | (t,id)::restid, v::restv -> (id,v) :: get_params restid restv
    | [],[] -> []
    | _ -> [])

let rec get_all_params (ct: class_table) (c:ident):(typ * ident) list =
  (match ct c with
    | Some Class(_,sc,ids,_) -> ids @ get_all_params ct sc 
    | _-> [])

(*Used to create new Object's instance vars -- ident * value list*)
let get_class_params (ct: class_table) (c: ident) (lv: value list) : (ident * value) list = 
  get_params  (rev (get_all_params ct c)) lv

let rec process_decls (dls: decl) (tc:tycon): tycon = 
  (match dls with
    | Decl(t,id) -> update tc id t
    | Dseq (d1,d2) -> process_decls d2 (process_decls d1 tc))

let rec make_env (li : ident list) (lv : value list) : env =
  match li, lv with
  | i :: irest, v :: vrest -> update (make_env irest vrest) i v
  | _, _ -> empty_state


let rec find_method (methods: mdecl list) (method_name: ident) : mdecl option =
  match methods with 
  | m :: rest -> (match m with
                | MDecl (_,name,_,_) -> if name = method_name then Some (m) else find_method rest method_name )
  | _ -> None

  let is_consistent (t1: typ) (t2: typ) : bool = 
    if t1 = t2 then true else (match t1, t2 with
                                | Dynamic, _ -> true
                                | _ , Dynamic -> true
                                | _,_ -> false)

let rec field_type (tidl:  (typ*ident)list) (id:ident) : typ = 
  (match tidl with
    | (t,i) :: rest -> if id = i then t else field_type rest id)

let find_type (e: exp) (tc: tycon) (ct: class_table) : typ =
  (match e with
  | Add(x,y) | Sub(x,y) | Mul(x,y) | Div(x,y) -> Tint
  | Int i -> Tint
  | Bool b -> TBool
  | And (e1, e2) | Or (e1,e2) | Eq(e1,e2) -> TBool
  | Var x -> (match tc x with
            | Some(t) -> t)

  | GetField(ex,id) -> (match ex with
                        | Var x -> (match tc x with
                                    | Some (Tclass c) -> (match ct c with 
                                        | Some Class (cl,scl,tidl,mlist) -> (field_type (rev (get_all_params ct c)) id)
                                        )
                                    )
                                    
                        )
  )

let rec methods (ct : class_table) (c : ident) : mdecl list =
  if c = "Object" then [] else
    match ct c with
    | Some (Class (_, d, _, lm)) -> methods ct d @ lm
    | _ -> []            

let lookup_method_aux (l : mdecl list) (m : ident) : mdecl option =
  find_opt (fun d -> match d with MDecl (_, n, _, _) -> n = m) l

let lookup_method ct c m =
  lookup_method_aux (rev (methods ct c)) m

let rec get_field_value (vl: (ident * value) list) (fld: ident) : value option =
  match vl with
  | (i,v) :: rest -> if i = fld then Some v else get_field_value rest fld
  | _ -> None

(* Used in SetField where the existing ident * value list needs to be updated with new ident and value*)  
let rec create_new_obj_arg_list (al: (ident * value) list) (new_val : value) (id: ident): (ident * value) list = 
  match al with
  | (i,v) :: rest -> if i = id then (i,new_val) :: rest else (i,v) :: create_new_obj_arg_list rest new_val id 
  | [] -> []

let id_exists (lv : (ident * value) list) (id: ident) : bool = 
  let filter_fun tup = match tup with
                      | (i,_) -> if i = id then true else false
  in 
  exists filter_fun lv
 
let field_exists (tidl : (typ * ident) list) (id: ident) (ty:typ): bool =
  let filter_fun tup = (match tup with
                      | (t,i) -> if (i = id && is_consistent ty t) then true else false) in
  exists filter_fun tidl

let get_type_of_value (v: value) : typ =
  match v with
  | IntV i -> Tint
  | BoolV b -> TBool

let rec typecheck_args (args1: exp list ) (args2: (typ * ident) list) (tc: tycon) (ct: class_table) : bool =
  match args1, args2 with
    | e1 :: rest_args, (t,id) :: rest_targs -> is_consistent (find_type e1 tc ct) t && typecheck_args rest_args rest_targs tc ct
    | [], [] -> true
    | _,_ -> false

let rec typecheck_exp (ct: class_table) (e: exp) (tc: tycon) (t: typ)  = 
  match e with
  | Int i -> is_consistent Tint t
  | Bool b -> is_consistent TBool t
  | Var id -> (match tc id with
                | Some tp -> is_consistent t tp
                | _ -> false)
  | Add (e1, e2) | Sub (e1, e2) | Mul (e1, e2) | Div (e1, e2) -> (typecheck_exp ct e1 tc Tint) && (typecheck_exp ct e2 tc  Tint ) && is_consistent Tint t
  | And (e1, e2) | Or (e1,e2) -> (typecheck_exp ct e1 tc TBool) && (typecheck_exp ct e2 tc TBool ) && is_consistent TBool t
  | Eq (e1 , e2) -> is_consistent TBool t && ((typecheck_exp ct e1 tc Tint && typecheck_exp ct e2 tc Tint) || (typecheck_exp ct e1 tc TBool && typecheck_exp ct e2 tc TBool)   ) 
  | GetField (e,id) -> (match e with
                        | Var x -> (match tc x with
                                    | Some Tclass c -> (match ct c with 
                                                      | Some Class (cl,scl,tidl,mlist) -> 
                                                        (if (field_exists ((rev (get_all_params ct c))) id t) then 
                                                                                (is_consistent t (field_type (rev (get_all_params ct c)) id)) 
                                                        else false)
                                                      | _ -> false)
                                    | Some Dynamic -> true
                                    | _ -> false)
                        | _ -> false)

let typecheck_ex (ct: class_table) (e : exp) (tc:tycon): bool = 
  (match e with
  | Add(e1,e2) | Sub(e1,e2) | Mul(e1,e2) | Div(e1,e2) -> typecheck_exp ct e tc Tint
  | Int i -> typecheck_exp ct e tc Tint
  | And(e1,e2) | Or(e1,e2) | Eq(e1,e2) -> typecheck_exp ct e tc TBool
  | Bool b -> typecheck_exp ct e tc TBool
  | GetField(ex,f) -> typecheck_exp ct e tc (find_type e tc ct)
  | Var x -> (match tc x with
              | Some t -> true
              | _ -> false) )

let rec typecheck_exps (ct: class_table) (exps: exp list) (tc: tycon): bool = 
  (match exps with 
    | ex :: rest -> typecheck_ex ct ex tc && typecheck_exps ct rest tc
    | [] -> true)

let rec eval_exp (ct: class_table) (e: exp) (s: state) (st : store) : value option = 
    match e with
    | Int i -> Some( IntV i)
    | Bool b -> Some( BoolV b)
    | Var id -> s id
    | Add(e1, e2) -> 
                  (match eval_exp ct e1 s st, eval_exp ct e2 s st with
                    | Some(IntV x) , Some(IntV y) -> Some(IntV (x+y))
                    | _,_ -> None )
    | Sub (e1,e2) -> (match eval_exp ct e1 s st, eval_exp ct e2 s st with
                    | Some(IntV x) , Some(IntV y) -> Some(IntV (x-y))
                    | _,_ -> None )
    | Mul (e1,e2) -> (match eval_exp ct e1 s st, eval_exp ct e2 s st with
                    | Some(IntV x) , Some(IntV y) -> Some(IntV (x*y))
                    | _,_ -> None )
    | Div (e1,e2) -> (match eval_exp ct e1 s st, eval_exp ct e2 s st with
                    | Some(IntV x), Some (IntV 0) -> None
                    | Some(IntV x) , Some(IntV y) -> Some(IntV (x/y))
                    | _,_ -> None )         
    | And (e1,e2) -> (match eval_exp ct e1 s st, eval_exp ct e2 s st with
                    | Some (BoolV a), Some(BoolV b) -> Some(BoolV (a && b))
                    | _,_ -> None)
    | Or (e1,e2) -> (match eval_exp ct e1 s st, eval_exp ct e2 s st with
                    | Some (BoolV a), Some(BoolV b) -> Some(BoolV (a || b))
                    | _,_ -> None)
    | Eq (e1,e2) -> (match eval_exp ct e1 s st, eval_exp ct e2 s st with
                    | Some (BoolV a), Some(BoolV b) -> Some(BoolV (a = b))
                    | Some (IntV i), Some(IntV j) -> Some(BoolV (i = j))
                    | _,_ -> None)
    | GetField (e,f) -> (match eval_exp ct e s st with
                    | Some (RefV pr) -> (match store_lookup st pr with
                      | Some (Obj (c,ivl) ) -> (match get_field_value ivl f with
                        | Some v -> Some (v)
                        | _ -> None)
                      | _ -> None)
                    | _ -> None)
    

let rec eval_exps (ct: class_table) (exps: exp list) (s: state) (st: store) : value list option =
  (match exps with
  | [] -> Some []
  | e :: rest -> (match eval_exp ct e s st, eval_exps ct rest s st with
                  | Some v , Some vs -> Some (v :: vs)
                  | _,_ -> None)
  |_ -> None)
                  

let rec eval_cmd (c: cmd) (ct: class_table) (st: store) (s: state) (tc: tycon) : (state * tycon * store) option = 
  match c with
  | Assign (id, e) -> 
    (match eval_exp ct e s st, tc id with
      | Some v, Some t -> if is_consistent (get_type_of_value v) t then Some((update s id v), tc, st ) else None 
      | _ -> None)
  | Seq (c1,c2) -> 
    (match eval_cmd c1 ct st s tc with
      | Some (s1, tc1, st1) -> eval_cmd c2 ct st1 s1 tc1
      | _ -> None )
  | SetField (e, f, e1) -> 
    (match eval_exp ct e s st, eval_exp ct e1 s st with
      | Some (RefV pr), Some (v) -> 
        (match store_lookup st pr with
          | Some Obj(c,lv) -> (match id_exists lv f with
                                | true -> Some(s,tc, update_store st pr (Obj (c, (create_new_obj_arg_list lv v f) )) )
                                | _ -> None)

          | _ -> None)
      | _ -> None)

  | IfC(e,c1,c2) -> (match eval_exp ct e s st with
                      | Some BoolV true -> eval_cmd c1 ct st s tc
                      | Some BoolV false -> eval_cmd c2 ct st s tc 
                      | _ -> None)
  
  | Invoke(x,e,m,args) -> (match eval_exp ct e s st, eval_exps ct args s st with
                          | Some (RefV pr), Some lv -> (match store_lookup st pr with
                                                        | Some Obj(c,_) -> (match lookup_method ct c m with
                                                            | Some (MDecl(_,_,params,body)) -> 
                                                                  (match eval_cmd body ct st (make_env ("this" :: map snd params) (RefV pr :: lv)) tc with
                                                                    | Some (env, _, _) -> 
                                                                      (match env "rvar", tc x with
                                                                        | Some v, Some t -> 
                                                                          (if is_consistent (get_type_of_value v) t then Some ( (update s x v),tc,st) else None)
                                                                        | _,_ -> None)
                                                                    | _ -> None)
                                                            | _ -> None)
                                                        | _ -> None)
                                                                       
                          | _ -> None)

  | Return e -> (match eval_exp ct e s st with
                | Some v -> Some( make_env ["rvar"] [v], tc, st)
                | _ -> None)

  | New (x,c,exps) -> (match eval_exps ct exps s st with
                      | Some (lv) -> 
                        let p = next_ref st in
                          Some (update s x (RefV p), tc, (update (store_lookup st) p (Obj (c, (get_class_params ct c lv))), p + 1))
                      | _ -> None);;


let rec typecheck_cmd (cd: cmd) (ct: class_table) (tc: tycon) : (bool*tycon) = 
  (match cd with
    | Assign(id,e) -> (match tc id, find_type e tc ct with
                      | Some t1, t2 -> if is_consistent t1 t2 then ((typecheck_exp ct e tc t2),tc) else (false,tc)
                      |_ -> (false,tc))
                      
    | Seq(c1,c2) -> (match typecheck_cmd c1 ct tc with
                      | (true,tc1) -> typecheck_cmd c2 ct tc1 
                      | false,tc1 -> false,tc1)
    
    | SetField (e, f, e1) -> (typecheck_exp ct (GetField(e,f)) tc (find_type e1 tc ct)),tc

    | IfC(e,c1,c2) -> (fst (typecheck_cmd c1 ct tc) && fst (typecheck_cmd c1 ct tc) && typecheck_exp ct e tc TBool),tc
    
    | Invoke(x,e,m,args) -> 
                    (match e with
                      | Var y -> (match tc y, tc x with
                                          | Some Tclass classt, Some t -> (match ct classt with
                                                    | Some Class (cl,scl,tidl,mlist) -> 
                                                          (match lookup_method ct cl m with
                                                          | Some MDecl(rt,m_name,params,body) -> ((is_consistent rt t ) && (typecheck_args args params tc ct)) && typecheck_exps ct args tc,tc
                                                          | _ -> false,tc)
                                                    | _ -> false,tc)  
                                          | _ -> false,tc )
                      | _ -> false,tc)

    | New (x,c,exps) -> 
                  (match ct c with
                    | Some Class(cl,scl,tidl,mlist) -> (typecheck_args exps (rev (get_all_params ct c)) tc ct) && typecheck_exps ct exps tc , update tc x (Tclass c)
                    | _ -> false,tc)

    (* Return statements can occur only inside MDecls which are in class table. 
       So, When a Mdecl is being typechecked, the body of Mdecl is typechecked with a 
       type context where r_type has the binding to the return type in Mdecl. So the 
       type of the expression in return command should be consistent to the return type passed/bound.
       We assume there are no stray return statements in the code.*)
    | Return e -> (match tc "r_type" with
                  | Some rt -> typecheck_exp ct e tc rt, tc
                  | _ -> false,tc)
    
    )

let rec update_tc (params: (typ * ident) list) (tc: tycon) : tycon = 
  (match params with
  | (t,id) :: rest ->  update_tc rest (update tc id t)
  | [] -> tc )

let typecheck_mdecl (ct: class_table) (m: mdecl) (tc:tycon) : bool = 
  (match m with
  | MDecl(t,id,params,body) -> (match typecheck_cmd body ct (update_tc (params @ [(t,"r_type")]) tc) with 
                                | (true,_) -> true 
                                | _ -> false ) )

let rec typecheck_mdecl_list (ct: class_table) (mlist : mdecl list) (tc: tycon) : bool =
  (match mlist with
  | m :: rest -> typecheck_mdecl ct m tc && typecheck_mdecl_list ct rest tc
  | [] -> true)

let rec typecheck_class (ct: class_table) (c: cdecl) (tc: tycon) : bool = 
  (match c with
  | Class (cl ,scl , tidl , mlist) -> typecheck_mdecl_list ct mlist (update tc "this" (Tclass cl))
  )

let rec typecheck_class_table (ct: class_table) (cls: ident list) (tc: tycon) : bool =
  (match cls with
  | c :: rest -> (match ct c with
                  | Some cl -> typecheck_class ct cl tc && typecheck_class_table ct rest tc
                  | _ -> false)
  | [] -> true)


  let tc = process_decls( Dseq(Decl(Dynamic, "x"),Decl(Dynamic, "house_price"))) empty_state;;



```