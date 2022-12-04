# TypeScript
## Basic Concept
1. interface vs type
	1. interface
    	- 확장 방법
      		```javascript
				interface PeopleInterface {
					name: string;
					age: number;
				}

				interface StudentInterface extends PeopleInterface {
					school: string;
				}    
      		```
		- 선언적 확장 가능	
			```javascript
				interface Window {
					title: string;
				}

				interface Window {
					ts: TypeScriptAPI;
				}

				// 같은 interface 명으로 Window를 다시 만든다면, 자동으로 확장이 된다.
			```
		- 객체에만 사용 가능
			```javascript
				interface FooInterface {
					value: string;
				}

				type FooType = {
					value: string;
				}

				type FooOnlyString = string;
				type FooTypeNumber = number;

				// 불가능
				interface X extends string {}
			```            
	1. type
		- 확장 방법
			```javascript
				type PeopleType = {
					name: string;
					age: number;
				}

				type StudentType = PeopleType & {
					school: string;
				}
			```
1. Utility Type
	1. Partial
    	- 특정 타입의 부분 집합을 만족하는 타입을 정의
      		```javascript
				interface Address {
					email: string;
					address: string;
				}

				type MyEmail = Partial<Address>;
				const me: MyEmail = {}; // 가능
				const you: MyEmail = { email: "noh5524@gmail.com" }; // 가능
				const all: MyEmail = { email: "noh5524@gmail.com", address: "secho" }; // 가능
      		```
  	1. Pick
    	- 특정 타입에서 몇 개의 속성을 선택해서 타입을 정의
			```javascript
				interface Product {
					id: number;
					name: string;
					price: number;
					brand: string;
					stock: number;
				}

				type shoppingItem = Pick<Product, 'id' | 'name' | 'price'>;

				const apple: Pick<Product, 'id' | 'name' | 'price'> = {
					id: 1,
					name: 'red apple',
					price: 1000,
				};
			```
	1. Omit
		- 특정 속성만 제거한 타입을 정의
			```javascript
				interface Product {
					id: number;
					name: string;
					price: number;
					brand: string;
					stock: number;
				}

				const apple: Omit<Product, 'stock'> = {
					id: 1,
					name: 'red apple',
					price: 1000,
					brand: 'del',
				};        
			```
1. Use Property Type Of Any Interface
	```javascript
		interface Meta {
			code: string;
			message: string;
		}

		interface Product {
			id: number;
			name: string;
			price: number;
			brand: string;
			stock: number;
		}

		interface ListOfProduct = {
			meta: Meta;
			data: Product[];
		};     

		const product: ListOfProduct['data'][0] = {
			id,
			name,
			price,
			brand,
			stock,
		}
	```