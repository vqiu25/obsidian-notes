# Purpose

The factory method is a creational pattern that delegates the responsibility of object instantiation to its concrete subclasses.

# UML Diagram

![[Drawing 2025-02-25 21.11.59.excalidraw | center]]

# Burger Example

## Creator

**~={purple}Abstract Class=~**
* **~={green}Pure Virtual Function=~**: Derived classes must provide an implementation. To override, 

```cpp
class BurgerCreator {
public:
	virtual ~BurgerCreator() {}
	
	virtual Burger* createBurger(BurgerType item) = 0;
	
	Burger* orderBurger(BurgerType type) {
		Burger* burger = createBurger(type);
		burger->prepare();
		burger->serve();
		return burger;
	}
};
```

**~={purple}Concrete Class=~**

```cpp
class EmilyBurgerStore : public BurgerCreator {
public:
	~EmilyBurgerStore() override {}
	
	Burger* createBurger(BurgerType item) override {
		if (item == BurgerType::CHEESE) {
			return new CheeseBurger();
		} else if (item == BurgerType::VEGAN) {
			return new VeganBurger();
		} else {
			return nullptr;
		}
	}
};
```

## Product

**~={purple}Enum for Product Types=~**

```cpp
enum class BurgerType {
	CHEESE, 
	VEGAN
};
```

**~={purple}Abstract Class=~** 
* Contains at least one virtual function
* **~={green}Virtual Function=~**: A function in the super class that is intended to be overridden in the sub class. We do this by adding `override` at the end of the method in the derived class.

```cpp
class Burger {
protected:
	string name;
	string bread;
	vector<string> toppings;
	
public:
	virtual void prepare() {}
	virtual void serve() {}
	
	string getName() {
		return this->name;
	}
};
```

**~={purple}Concrete Class=~**

```cpp
class CheeseBurger : public Burger {
public:
	CheeseBurger() {
		this->name = "Cheese Burger";
		this->bread = "Brioche";
		this->toppings = {"Cheese", "Sauce", "Lettuce"};
	}
	
	void prepare() override { cout << "Preparing" << endl; }
	void serve() override { cout << "Serving" << endl; }
};

class VeganBurger : public Burger {
public:
	VeganBurger() {
		this->name = "Vegan Burger";
		this->bread = "Brioche";
		this->toppings = {"Cheese", "Sauce", "Lettuce"};
	}
	
	void prepare() override { cout << "Preparing" << endl; }
	void serve() override { cout << "Serving" << endl; }
};
```

## Main

```cpp
int main() {
	BurgerCreator* emilyStore = new EmilyBurgerStore();
	Burger* burger = emilyStore->orderBurger(BurgerType::CHEESE);
	cout << "We ordered a " << burger->getName();
	
	delete emilyStore;
	delete burger;
}
```


**~={green}Constructor and Destructor Behaviour=~**

If stack allocated, and deallocated, this is the order which follows:

1. Base constructor
2. Derived constructor
3. Derived destructor
4. Base destructor 

If heap allocated via pointers, this is the order which follows without virtual destructors:

1. Base constructor
2. Derived constructor
3. Base destructor
4. ❌ (Derived destructor is skipped!)

As such, we should do the following:

![[Pasted image 20250226081315.png | center | 450]]
