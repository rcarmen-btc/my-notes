# JS OOP
```js
function PartyAnimal(inition) {
	this.x = inition;
	this.party = function () {
		this.x += this.x;
		console.log('So far' + this.x);
	}
}

an = new PartyAnimal();

an.party();
an.party();
an.party();
```