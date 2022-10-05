## ex00:
	class Warlock ortodox;
		-	copy constructor and = privat 
		- 	default constructor and destructor
		-	getters and setter
		-	intro

## ex01:
	class Warlock ortodox;
		-	copy constructor and = privat 
		- 	default constructor and destructor
		-	getters and setter
		-	intro
		-	brain(name, spell)
		-	lern, forget, launch(name, target);
	
	abstract class ASpell ortodox;
		- name, effects with getters;
		- clone pure method;
		- all can be called on a const obj;
		- (name, effects) constr;
		- launch(ATarget const &t); 
	class Fwoosh;
		- destructor, default, clone
	
	abstract class ATarget;
		- (type) constr with getter;
		- clone pure method;
		- all can be called on a const obj;
		- getHitBySpell(ASpell const &spell);	
	class Dummy;
		- destructor, default, clone


## ex02
	class Warlock ortodox;
		-	copy constructor and = privat 
		- 	default constructor and destructor
		-	getters and setter
		-	intro
		-	brain(name, spell)
		-	lern, forget, launch(name, target);
	class SpellBook;
		-	copy constructor and = privat 
		- 	default constructor and destructor
		-	data(name, spell)
		-	lern, forget, create(name);

================
	
	abstract class ASpell ortodox;
		- name, effects with getters;
		- clone pure method;
		- all can be called on a const obj;
		- (name, effects) constr;
		- launch(ATarget const &t); 
	class Fwoosh;
		- destructor, default, clone
	class Fireball;
	class Polymorph;

===============
	
	abstract class ATarget;
		- (type) constr with getter;
		- clone pure method;
		- all can be called on a const obj;
		- getHitBySpell(ASpell const &spell);	
	class Dummy;
		- destructor, default, clone
	class BrickWall;
	Class TargetGen;
		-	lern, forget, create(name);

		
