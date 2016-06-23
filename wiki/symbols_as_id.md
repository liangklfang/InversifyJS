# Support for Symbols
In very large applications using strings as the identifiers of the types to be injected by the InversifyJS can lead to naming collisions. InversifyJS supports and recommends the usage of [Symbols](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol) instead of string literals.

> A symbol is a unique and immutable data type and may be used as an identifier for object properties. The symbol object is an implicit object wrapper for the symbol primitive data type.

```ts
import { Kernel, injectable, inject } from "inversify";

let Symbols = {
	Ninja : Symbol("Ninja"),
	Katana : Symbol("Katana"),
	Shuriken : Symbol("Shuriken")
};

@injectable()
class Katana implements Katana {
    public hit() {
        return "cut!";
    }
}

@injectable()
class Shuriken implements Shuriken {
    public throw() {
        return "hit!";
    }
}

@injectable()
class Ninja implements Ninja {

    private _katana: Katana;
    private _shuriken: Shuriken;

    public constructor(
	    @inject(Symbols.Katana) katana: Katana,
	    @inject(Symbols.Shuriken) shuriken: Shuriken
    ) {
        this._katana = katana;
        this._shuriken = shuriken;
    }

    public fight() { return this._katana.hit(); };
    public sneak() { return this._shuriken.throw(); };

}

var kernel = new Kernel();
kernel.bind<Ninja>(Symbols.Ninja).to(Ninja);
kernel.bind<Katana>(Symbols.Katana).to(Katana);
kernel.bind<Shuriken>(Symbols.Shuriken).to(Shuriken);
```