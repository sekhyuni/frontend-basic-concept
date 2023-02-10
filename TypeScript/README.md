# TypeScript

* [Type Options](#type-options)
* [interface vs type](#interface-vs-type)
* [Utility Type](#utility-type)
* [Index Signature](#index-signature)
* [Use Property Type Of Any Interface](#use-property-type-of-any-interface)
* [Generic](#generic)
* [Readonly](#readonly)

## Type Options
|Type|JS|TS|Description|
|:---:|:---:|:---:|:---:|
|boolean|O|O|true와 false|
|null|O|O|값이 없다는 것을 명시|
|undefined|O|O|값을 할당하지 않은 변수의 초기값|
|number|O|O|숫자(정수와 실수, Infinity, NaN|
|string|O|O|문자열|
|symbol|O|O|고유하고 수정 불가능한 데이터 타입이며 주로 객체 프로퍼티들의 식별자로 사용(ES6에서 추가)|
|object|O|O|객체형(참조형)|
|array|X|O|배열|
|tuple|X|O|고정된 요소수 만큼의 타입을 미리 선언후 배열을 표현|
|enum|X|O|열거형. 숫자값 집합에 이름을 지정한 것|
|any|X|O|타입 추론(type inference)할 수 없거나 타입 체크가 필요없는 변수에 사용. var 키워드로 선언한 변수와 같이 어떤 타입의 값이라도 할당 가능|
|void|X|O|일반적으로 함수에서 반환값이 없을 경우 사용|
|never|X|O|결코 발생하지 않는 값|

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#typescript)
## interface vs type
1. interface
	- 확장 방법
		```typescript
		interface PeopleInterface {
			name: string;
			age: number;
		}

		interface StudentInterface extends PeopleInterface {
			school: string;
		}    
		```
	- 선언적 확장 가능	
		```typescript
		interface Window {
			title: string;
		}

		interface Window {
			ts: TypeScriptAPI;
		}

		// 같은 interface 명으로 Window를 다시 만든다면, 자동으로 확장
		```
	- 객체에만 사용 가능
		```typescript
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
		```typescript
		type PeopleType = {
			name: string;
			age: number;
		}

		type StudentType = PeopleType & {
			school: string;
		}
		```

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#typescript)
## Utility Type
1. Partial
	- 특정 타입의 부분 집합을 만족하는 타입을 정의
		```typescript
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
		```typescript
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
		```typescript
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

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#typescript)
## Use Property Type Of Any Interface
```typescript
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
};
```
	
[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#typescript)
## Index Signature
- interface 또는 객체형 type의 속성 이름은 알지 못하고 값의 타입은 알고 있을 때 사용
- key의 타입은 string, number, symbol, template literal만 가능
	```typescript
	type UserType = {
		[key: string]: string | number | boolean;
	};

	const user: UserType = {
		name: '홍길동',
		age: 20,
		man: true,
	};
	```

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#typescript)
## Generic
- 제네릭은 선언 시점이 아니라 생성 시점에 타입을 명시하여 하나의 타입만이 아닌 다양한 타입을 사용할 수 있도록 하는 기법
- 한번의 선언으로 다양한 타입에 재사용 가능
- T는 제네릭을 선언할 때 관용적으로 사용되는 식별자로 타입 파라미터(Type parameter)를 의미
	```typescript
	const doSomething = <T extends string>(param: T): T => {
		return param;
	};

	doSomething('a');
	```

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#typescript)
## Readonly
1. readonly
	```typescript
	type SomeType = {
		readonly prop: number;
	};
	interface SomeInterface {
		readonly prop: number;
	}

	const objOfSomeType: SomeType = { prop: 1 };
	const objOfSomeInterface: SomeInterface = { prop: 1 };

	objOfSomeType.prop = 2; // Error. 읽기 전용 속성이므로 'prop'에 할당할 수 없습니다.
	objOfSomeInterface.prop = 2; // Error. 읽기 전용 속성이므로 'prop'에 할당할 수 없습니다.
	```
1. Readonly
	```typescript
	type SomeType = {
		prop: number;
	};
	type SomeTypeReadonly = Readonly<SomeType>;

	const objOfSomeType: SomeType = { prop: 1 };
	const objOfSomeTypeReadonly: SomeTypeReadonly = { prop: 1 };

	objOfSomeType.prop = 2; // OK
	objOfSomeTypeReadonly.prop = 2; // Error. 읽기 전용 속성이므로 'prop'에 할당할 수 없습니다.
	```
1. ReadonlyArray
	```typescript
	const listOfStringUseReadonlyArray: ReadonlyArray<string> = ['a'];
	const listOfStringUseReadonly: readonly string[] = ['a'];

	listOfStringUseReadonlyArray.push('b'); // Error. 'readonly string[]' 형식에 'push' 속성이 없습니다.
	listOfStringUseReadonly.push('b'); // Error. 'readonly string[]' 형식에 'push' 속성이 없습니다.
	```
1. const Type Parameters
	- Version < 5.0
		```typescript
		const outerFunction = <T extends readonly string[]>(outerParam: T) => {
			const innerFunction = (innerParam: T) => {};
			return { innerFunction };
		};

		outerFunction(['a'] as const).innerFunction(['a', 'b']); // Error. '["a", "b"]' 형식의 인수는 'readonly ["a"]' 형식의 매개 변수에 할당될 수 없습니다. 소스에 2개 요소가 있지만, 대상에서 1개만 허용합니다.
		```
	- Version >= 5.0
		```typescript
		const outerFunction = <const T extends string[]>(outerParam: T) => {
			const innerFunction = (innerParam: T) => {};
			return { innerFunction };
		};

		outerFunction(['a']).innerFunction(['a', 'b']); // Error. '["a", "b"]' 형식의 인수는 'readonly ["a"]' 형식의 매개 변수에 할당될 수 없습니다. 소스에 2개 요소가 있지만, 대상에서 1개만 허용합니다.
		```

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#typescript)