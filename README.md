<!--

author:   Andre Dietrich
email:    dietrich@ivs.cs.uni-magdeburg.de
version:  1.0.0
language: en_US
narrator: US English Female

script:   https://cdn.rawgit.com/liaScript/tau-prolog_template/master/js/tau-prolog.js

@tau_prolog_program
<script>
    window['@0'] = {session: window.pl.create(), query: null, rslt: "", query_str: ""};
    var c = window['@0']['session'].consult(`{X}`);
    if( c !== true )
        throw {message: 'parsing program => ' + c.args[0]};
    else
        "database loaded";
</script>
@end

@tau_prolog_query
<script>
    var query = `{X}`;

    if(window['@0']['query'] == null || window['@0']['query_str'] != query) {
        window['@0']['query_str'] = query;
        window['@0']['rslt'] = "";
        window['@0']['query'] = window['@0']['session'].query(query);
    }

    if( window['@0']['query'] !== true ) {
        throw {message: 'parsing query => ' + window['@0']['query'].args[0]};
    }
    else {
        var callback = function(answer) {
            window['@0']['rslt'] +=  window.pl.format_answer( answer ) + "<br>";
        };
        window['@0']['session'].answer(callback);

        window['@0']['rslt'];
    }
</script>
@end


@tau_prolog
```prolog
@2
```
@tau_prolog_program(@0)


```prolog
@1
```
@tau_prolog_query(@0)
@end

-->

# Arbeitsbuch PROLOG

Template for integrating the Tau-Prolog interpreter into LiaScript

## Plain HTML

** Load Database and Rules: **

```prolog
likes(sam, salad).
likes(dean, pie).
likes(sam, apples).
likes(dean, whiskey).
```
<script>
    window["session_name"] = {session: window.pl.create(), query: null, rslt: "", query_str: ""};
    var c = window["session_name"]['session'].consult(`{X}`);
    if( c !== true )
        throw {message: 'parsing program => ' + c.args[0]};
    else
        "database loaded";
</script>

** Queries **

```prolog
likes(sam, X).
```
<script>
var query = `{X}`;

if(window['session_name']['query'] == null || window['@session_name']['query_str'] != query) {
    window['session_name']['query_str'] = query;
    window['session_name']['rslt'] = "";
    window['session_name']['query'] = window['session_name']['session'].query(query);
}

if( window['session_name']['query'] !== true ) {
    throw {message: 'parsing query => ' + window['session_name']['query'].args[0]};
}
else {
    var callback = function(answer) {
        window['session_name']['rslt'] +=  window.pl.format_answer( answer ) + "<br>";
    };
    window['session_name']['session'].answer(callback);

    window['session_name']['rslt'];
}
</script>


## Macros-Separated

** Load Database and Rules: **

```prolog
likes(sam, salad).
likes(dean, pie).
likes(sam, apples).
likes(dean, whiskey).
```
@tau_prolog_program(session_name)

** Queries **

```prolog
likes(sam, X).
```
@tau_prolog_query(session_name)


## Full-Macro

```prolog
@tau_prolog(session_nameX,`likes(sam, X).`)
likes(sam, salad).
likes(dean, pie).
likes(sam, apples).
likes(dean, whiskey).
```
