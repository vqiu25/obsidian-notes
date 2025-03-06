# Purpose

The Singleton is a creational pattern that ensures a class has at most one instance and provides a global point of access to this instance.

# UML Diagram

![[Drawing 2025-02-25 22.48.48.excalidraw | center]]

# Printer Service Example

## Singleton 

**~={purple}Details=~**
* **~={red}Private Constructor=~**: The singleton makes use of a private constructor. This way, we restrict the instantiation of the class to be within the class.
* **~={red}Static Variable of it's own Type=~**: Often dubbed `uniqueInstance`, this component preserves the sole instance of the Singleton. Initialised to null, it only gets its assignment when the `getInstance()` method is invoked for the first time.
* **~={red}Public Static Get Instance=~**: This method acts as the gateway to the Singleton's exclusive instance. It checks if `uniqueInstance` is null; if so, it instantiates it. Otherwise, it simply returns the pre-existing instance, maintaining that at most one instance of the Singleton class permeates the system.

**~={purple}C++=~**
* **~={green}Static Variable and Static Methods=~**:

![[Drawing 2025-02-26 09.29.12.excalidraw | center | 700]]

```cpp
class PrinterService {
private:
	static PrinterService* uniqueInstance;
	static mutex mtx; 
	string type;

	PrinterService(string type) : type{type} {}
	
public:
	// The copy constructor and the assignment operator 
	// are deleted to prevent copying of the singleton instance.
    PrinterService(const PrinterService &) = delete;
    PrinterService &operator=(const PrinterService &) = delete;

	static PrinterService* getInstance() {
		if (uniqueInstance == nullptr) {
			lock_guard<mutex> lock{mtx};
			
			if (uniqueInstance == nullptr) {
				uniqueInstance = new PrinterService{"Colour"};
			}
		}
		return uniqueInstance;
	}

	string getType() {
		return type;
	}

	void setType(string newType) {
		type = newType;
	}
};
```

## Client (Main) 

**~={purple}Static Variables=~**
* Static Variables must be defined outside of the class
* `::` is used to access **static members** or static methods or **namespaces**.

```cpp
PrinterService* PrinterService::uniqueInstance = nullptr;
mutex PrinterService::mtx;

int main() {
	PrinterService* printerState = PrinterService::getInstance();
	printerState->setType("GrayScale");
}
```
