# React Life Cycle

## Diagram of the execution order of the methods

![graph](./images/graph.png)

## Initialization

Đây là giai đoạn mà **component** sẽ bắt đầu hành trình của nó bằng cách thiết lập **state** và **props**. Điều này thường được thực hiện bên trong **constructor method**.

```javascript
class Initialize extends React.Component {
    constructor(props){
        // Calling the constructor of
        // Parent Class React.Component
        super(props);
        // initialization process
        this.state = {
            date : new Date(),
            clickedStatus: false,
        };
    }
    // code below
```

## Mounting

- **Mounting** là giai đoạn mà **React component** của chúng ta sẽ được **mount on the DOM** (được khởi tạo và chèn vào cây dom).

- Giai đoạn này được thực hiện sau khi giai đoạn **initialization** được hoàn thành. Trong giai đoạn này, **component** sẽ được gọi lần đầu tiên.

### 1. ComponentWillMount()

- **Method** này sẽ được gọi ngay trước khi **component** được **mount on the DOM** hoặc **render method** được gọi.

> Lưu ý: Bạn không nên thực hiện một cuộc gọi **API** hoặc dùng `this.setState` để thay đổi bất kì dữ liệu nào trong **method** này. Bởi vì nó sẽ được gọi trước khi **render component**. Vì vậy, bạn không thể **update state** với sự trả về của **API**.

### 2. ComponentDidMount()

- **Method** này sẽ được gọi sau khi **component** được **mount on the DOM**. Giống như `componentWillMount`, nó sẽ được gọi một lần trong một **lifecycle**.

- Trước khi thực thi **method** này, thì **render method** sẽ được gọi. Chúng ta có thể thực hiện một cuộc gọi **API** hoặc **update state** với **API response** ở trong **method** này.

### 3. Example

```javascript
class LifeCycle extends React.Component {
	componentWillMount() {
		console.log('Component will mount!');
	}
	componentDidMount() {
		console.log('Component did mount!');
		this.getList();
	}
	getList = () => {
		/*** method to make api call***/
	};
	render() {
		return (
			<div>
				<h3>Hello mounting methods!</h3>
			</div>
		);
	}
}
```

## Updating

- Đây là giai đoạn thứ ba trong mỗi **lifecycle**. Sau giai đoạn **mounting** (nơi mà các **component** được tạo ra), đây là giai đoạn **updating** bắt đầu thực hiện. Đây là nơi **state** của **component** thay đổi, và việc **re-rendering** được diễn ra.

- Trong giai đoạn này, dữ liệu của mỗi **component(state & props)** sẽ được **update** khi **user** thực hiện các hành động như `click`, `typing`… Kết quả sẽ **re-rendering** lại **component**.

### 1. ShouldComponentUpdate()

- **Method** này sẽ xác định rằng **component** có được **update** hay là không. Mặc định sẽ trả về là **true**. Nhưng đến một lúc nào đó, nếu bạn muốn **re-render** lại **component** bằng một số điều kiện ràng buộc, thì viết các điều kiện đó trong **method** ràng buộc này là một điều lý tưởng.

- Giả sử, bạn chỉ muốn **re-render component** khi `prop` thay đổi. Method này sẽ nhận các đối số như `nextProps` và `nextState` để giúp chúng ta quyết định một cách dễ dàng hơn khi so sánh với `props` hiện tại.

### 2.ComponentWillUpdate()

- **Method** này sẽ được gọi trước khi **component update** và sẽ được gọi mỗi lần sau `shouldComponentUpdate`.
- Nếu bạn muốn thực hiện một số phép tính trước khi **re-render component** và sau khi **update** `state` và `props`, thì đây là một **method** hợp lý để thực hiện. Như `shouldComponentUpdate`, nó sẽ nhận các đối số như `nextProps` và `nextState`.

### 3. ComponentDidUpdate()

- **Method** này sẽ chỉ được gọi sau khi **re-render component**. Mỗi lần **update** mới, **component** sẽ được **update** vào **DOM**, sau đó `componentDidUpdate` sẽ được thực hiện.

- **Method** này sẽ nhận vào các đối số như `prevProps` và `prevState`.

### 4. Example

```javascript
class LifeCycle extends React.Component {
	constructor(props) {
		super(props);
		this.state = {
			date: new Date(),
			clickedStatus: false,
			list: [],
		};
	}
	componentWillMount() {
		console.log('Component will mount!');
	}
	componentDidMount() {
		console.log('Component did mount!');
		this.getList();
	}
	getList = () => {
		fetch('https://api.mydomain.com')
			.then((response) => response.json())
			.then((data) => this.setState({ list: data }));
	};
	shouldComponentUpdate(nextProps, nextState) {
		return this.state.list !== nextState.list;
	}
	componentWillUpdate(nextProps, nextState) {
		console.log('Component will update!');
	}
	componentDidUpdate(prevProps, prevState) {
		console.log('Component did update!');
	}
	render() {
		return (
			<div>
				<h3>Hello Mounting Lifecycle Methods!</h3>
			</div>
		);
	}
}
```

## Unmounting

Đây là giai đoạn cuối cùng của một **component lifecycle**. Như cái tên của nó, chúng ta có thể hiểu một cách rõ ràng. Nó sẽ **unmounted DOM** trong giai đoạn này.

### 1. ComponentWillUnmount()

- **Method** này sẽ được gọi trước khi **unmmount component**. Trước khi loại bỏ thành phần khỏi **DOM**, `componentWillUnmount` sẽ được thực thi.
- **Method** này biểu thị sự kết thúc của một **lifecycle**.

## Reference article

[CodersX](https://coders-x.com/2019/06/15/Chi-tiet-ve-lifecycle-method-trong-ReactJS/)
