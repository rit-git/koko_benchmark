## Type 1: Entities
### Q1: Extract entities that are delicious;
        extract e:Entity, d:Str from input if
            (/root:{
                a = //verb,
                b = a/dobj,
                c = b//”delicious”
            } (b) in (e), (d) eq (b.subtree)) 
### Q2: Extract cafe names;
        extract x:Entity from input if () satisfying
                   (x ", a full" {0.1}) or
                   ("introducing" x {0.1}) or
                   (str(x) contains "Cafe" {0.2})
        with threshold 0.0
        
## Type 2: Happy moments
### Q3: Extract activities that make people feel happy;
        extract a:Str, c:Str from input if
            (/root:{
                a = //verb,
                b = a/dobj,
                c = b.subtree
            }) satisfying c
            (c near "happy" {1})
            
## Type 3: Information extraction
### Q4: Extract is-locate relationship;
        extract a:gpe, b:gpe from input if
            (/root: {
                 v = //verb
            } v: {
                 c = a + ^ + v + ^ + b
            }) satisfying v
            (str(v) ~ "located" {1})

### Q5: Extract title or nicknames for entities;
        extract a:Entity, b:Str from input if
            (/root:{
            	v = //verb,
            	p = //verb/propn,
            	b = p.subtree,
            	c = a + ^ + v + ^ + b
            }) satisfying v
            (str(v) ~ "called" {1})

### Q6: Extract chocolate types;
        extract c:Entity from input if
            (/root:{
             v = //verb,
             o = v//pobj[@text="chocolate"],
             s = v/nsubj
            } (s) in (c)) satisfying v
            (str(v) ~ "is" {1})

### Q7: Extract date of birth for people.
        extract a:Person, b:Date from input if 
                (/root: {
                v = //verb
                }) satisfying v
		            (str(v) ~ "born" {1})
                
## Type 4: Answers for Wh-queries
### Q8: Extract answer for who is Cinderella;
        extract c:Str from input if
            (/root:{
            	v = //verb,
            	s = v/"Cinderella",
            	c = v + ^ + /v/noun
            }) satisfying v
            (str(v) ~ "is" {1})

### Q9: Extract answer for who created chocolate;
        extract c:Entity from input if
            (/root:{
            	v = //verb,
            	s = v/nsubj,
            	b = v/"chocolate"
            } (s) in (c)) satisfying v
            (str(v) ~ "create" {1})


### Q10: Extract answer for Which planet is called the red planet.
        extract a:Entity from input if
            (/root:{
            	v = //verb,
            	s = v/nsubjpass
            } v: {
            	k = "Red Planet"
            } (s) in (a)) satisfying v
            (str(v) ~ "called" {1})        
