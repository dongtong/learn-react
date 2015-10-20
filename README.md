# React学习笔记

##谁在用React

- facebook
- Instagram


facebook.github.io/react

使用react 0.13.2创建class使用createClass取代createComponent

transferProps方法被废除了

## 优势

- 速度: 
	- 60fps渲染页面
	- render < 16ms(使用noverl random 算法)
	
- 声明式绑定(binding)
- 组合


## 架构

props----->|        |------>|     |
			render			DOM

state----->|        |------>|     |

props和state的区别是state可以改变。所以在设计component时，不要使用state.

用户操作DOM，引发state变化，然后重新渲染(render)虚拟DOM, 而不是真实DOM, React判断虚拟DOM和
真实DOM的差异，更新真实DOM。这样可以提高更新DOM效率。

Model + Component = DOM


/** @jsx React.DOM **/现在不是必需的

##与其他框架的对比

- Anguarl和Ember框架太大，提供了SPA的基本所有功能。
- 一般React结合Backbone使用，使用React作为Backbone的View
- 与knockout相比，都提供了render Model to DOM和事件处理机制。

##什么是React.js

- react.js库用来构建客户端用户界面，开源的，由facebook维护。
- 针对MVC中的V(View)
- 用它来构建大型的可伸缩的SPA(单页面应用程序)。
- 最大的亮点是使用高速虚拟DOM(high-speed virtual DOM)
- 使用简洁易懂的JSX语法

##为什么React.js如此之快?

它渲染DOM速度快。

- 读写JavaScript对象比读写DOM对象快
- React虚拟DOM其实就是JavaScript对象
- React不会读取“真实”的DOM，仅仅会在需要的时候写真实DOM
- React处理更新DOM的效率高

正常情况下，在需要情况下读区DOM, 在需要情况下再写入DOM.Backbone的做法是不停地渲染DOM(可以理解为写行为),这样会导致页面更新慢。React的做法是:JS只与虚拟DOM交互，如果需要获取DOM,从虚拟DOM获取，如果渲染的话，更新虚拟DOM,虚拟DOM只会根据需要更新部分真实的DOM。


##工具

chrome:
- react-detector
- react developer tools


## 入门

https://facebook.github.io/react/index.html

	<!DOCTYPE html>
	<html>
	<head>
		<title>第一个React</title>
		<script src="js/react.min.js"></script>
	</head>
	<body>
		<script>
		//创建一个div DOM,没有属性，内容是Hello React, 放在document.body DOM下面
		React.render(React.createElement('div', null, "Hello React"), document.body);
		</script>
	</body>
	</html>
	
创建一个DOM可以使用以上语法，如果创建许多DOM,可以使用JSX。浏览当识别text/jsx时，将会把其中的内容转化为纯JavaScript对象。

	<!DOCTYPE html>
	<html>
	<head>
		<title>第一个React</title>
		<script src="js/react.min.js"></script>
		<script src="js/JSXTransformer.js"></script>
	</head>
	<body>
		<script type="text/jsx">
			var HelloWorld = React.createClass({
				render: function(){
					return (
						<div>
							<h1>Hello World</h1>
							<p>I am using React JavaScript Library</p>
						</div>
					);
				}
			})
		
		React.render(<HelloWorld />, document.body);
		</script>
	</body>
	</html>
	
JSXTransformer.js将JSX内容在浏览器运行时，转化为纯JavaScript。如果每次都这么做，会让程序变得慢。如果在浏览器运行之前，将JSX转化，将会变得快起来。首先将jsx内容放到一个js文件中

	var HelloWorld = React.createClass({
				render: function(){
					return (
						<div>
							<h1>Hello World</h1>
							<p>I am using React JavaScript Library</p>
						</div>
					);
				}
			})
		
		React.render(<HelloWorld />, document.body);
		
下面使用npm包装react-tools库，将jsx转化为JavaScript.

	sudo npm install -g react-tools
	
然后使用以下命令，将jsx转化为javascript

	jsx src/ build/
	built Module("helloworld")
	["helloworld"]
	
最后链接转化后的javascript,去掉<script src="js/JSXTransformer.js"></script>，因为不需要在浏览器段转化，去掉text/jsx,因为已经使用转化后的javascript

	<!DOCTYPE html>
	<html>
	<head>
		<title>第一个React</title>
		<script src="js/react.min.js"></script>
	</head>
	<body>
		<script src="build/helloworld.js">
		</script>
	</body>
	</html>
	
## JSX

JSX不是React必须的。

在线体验: https://facebook.github.io/react/jsx-compiler.html

- javascript中支持类似于xml语法。
- 每一个元素会被转化成JavaScript函数调用。

		var Hello = <Hello>World</Hello>;
		＝》
		var Hello = Hello(null, "World");
		
		var Hello = <Hello name="foobar" age="28">World</Hello>;
		＝》
		var Hello = Hello({name: "foobar", age: "28"}, "World");

		var Hello = <Hello name="foobar" age="28"><World /></Hello>;
		＝》
		var Hello = Hello({name: "foobar", age: "28"}, World(null));

- 关注点分离

生成HTML标签代码和HTML标签紧密联系，标签与业务逻辑相互分离。让处理业务逻辑部分更加清晰。将每一个关注点创造一个组件，然后将所有的逻辑以及标签封装在里面。

- 抽象化

使用JSX，方便以后React版本迁移，不需要改代码或者少改代码

- 更加直观

JSX语法描述HTML组件树更加直观

- 更加语义化
- 原来的语法有改变

		React.DOM.xx
		=>
		React.createElement("xx", null)
		
- 在线转化JSX有以下缺陷

	- 不能压缩
	- 不能linted
	- 不容易在浏览器中debug
	－ 无法JavaScript语法高亮
	
- JSX预处理

将JSX转化为纯JavaScript，加载速度比实时编译的速度快很多，可以使用react-tools node模块

	sudo npm install -g react-tools
	jsx src/ build/
	jsx --watch src/ build/

- 访问child

以下可以访问子组件

		this.props.children

- JSX之间的空白符是没有用的，不会处理

- 给JSX DOM设置属性需要遵循HTML规约，然后会传给render,自定义属性需要加上data-, 不能使用JavaScript保留字

		<div data-customArr="foobar" />

如果有保留字，如for

		<label htmlFor="name" className="big" />
		
- JSX中的style属性，是一个JavaScript对象，使用camel记法

	  var style = {
		  backgroundColor: '#eee',
		  border: '1px solid #ddd'
	  };
	
	  return <div style={s}>test</div>

- 转义

react默认对所有内容进行转义。	对html内容进行转义：

	return <p dangerouslySetInnerHTML={{__html: this.props.dangerous}}></p>
	
	React.render(<Hello name="<strong>World</strong>" />, document.body);
	
	
- 属性值可以是字符串，或者JavaScript表达式


## React Component

用户可以自定义react component， 用于界面UI重用。组件可以创建得比较大，也可以是一小部分。下面创建一个component,如何重用component.

	<!DOCTYPE html>
	<html>
	<head>
		<title>第一个React</title>
		<script src="js/react.min.js"></script>
		<script src="js/JSXTransformer.js"></script>
	</head>
	<body>
		<div id="1_component"></div>
		<div id="many_component"></div>
		<script type="text/jsx">
			var MyComponent = React.createClass({
				render: function(){
					return <div>My First Component!</div>;
				}
			});
			React.render(<MyComponent />, document.getElementById('1_component'));
			React.render(<div class="many_components">
				<MyComponent />
				<MyComponent />
				<MyComponent />
			</div>, 
			document.getElementById('many_component'));
		</script>
	</body>
	
这里需要注意的是，组件多次重用时，需要一个wrapper包装。

props可以动态的访问component中的属性数据,重用一次component,相当于实例化一个对象，而createClass相当于创建一个类，而render类似于类的初始化函数，这里一般是在初始化component的模板。

可以通过props访问children内容, 最大化component重用。

	<script type="text/jsx">
			var MyComponent = React.createClass({
				render: function(){
					return <div>
					<h1>{this.props.text}</h1>
					<p>{this.props.children}</p>
					</div>;
				}
			});
			//React.render(<MyComponent />, document.getElementById('1_component'));
			React.render(<div class="many_components">
				<MyComponent text="Hello">This is Hello</MyComponent>
				<MyComponent text="React">This is React</MyComponent>
				<MyComponent text="I use U">I am trying you!</MyComponent>
			</div>, 
			document.getElementById('many_component'));
		</script>
		
对于null和false react不会输出任何内容。


一般component结构app.js

	(function(){
		'use strict';
		
		var ComponentName = React.createClass({
			//...
		});
		
		React.render(<ComponentName />, DOM)
	})();


##组件的生命周期

React组件就是一个状态机。如果state发生改变，将会有新的state流入组件树。

- 实例化(Mounting)

hook方法有

	getDefaultProps
	getInitialState
	componentWillMount
	render
	componentDidMount
	
getDefaultProps只会被调用一次，没有被父组件指定props属性的新实例，用此方法可以给实例设置默认的props值。在React.createClass时就被调用。下面就是将inline-style定义在getDefaultProps方法中，这样可以重用inline-style, 这里的inline-style是在js中维护的:

	var SimpleDiv = React.createClass({
		getDefaultProps: function() {
			return {
				backgroundColor: 'green',
				height: 200,
				width: 200
			}
		},
		
		render: function() {
			return (<div style={this.props}/>);
		}
	});
	
	React.render(<SimpleDiv />, document.body);
	
getInitialState每次组件实例化之前被调用一次，这是与getDefaultProps不同之处。在这里可以访问this.props

componentWillMount是在组件渲染之前可以修改组件state的最后一次机会。
	
render方法必须满足

1. 只能通过this.props和this.state访问数据
2. 可以返回null, false或者任何React组件。
3. 只能出现一个顶级组件(不能返回一组元素)
4. 必须纯净，不能改变组件的状态或者修改DOM的输出
	
render方法返回的不是真实的DOM,是虚拟DOM, React会比对虚拟DOM和真实DOM,来决定是否需要更新真实DOM。可以在componentDidMount中调用this.getDOMNode()来获取真实DOM,然后运用一些jQuery插件。但是如果React运行在服务端时componentDidMount方法将不会被调用。
	
getDefaultProps在组件首次创建实例时会有，后面再创建实例时将从getInitialState开始。

componentDidMount只会为每个DOM调用一次，所以不用担心在这里定义的方法会被多次调用，产生副作用。

**为了保证组件被移除后，可能导致内存泄漏或者其他问题，如果组件实现了componentDidMount方法，那么最好实现componentWillUnmount方法，移除DOM.如果使用了一些插件，那么要看看插件有没有定义全局变量，定时器，或者如何卸载插件**

- 存在期(Updating)

hook方法有

	componentWillReceiveProps
	shouldComponentUpdate
	componentWillUpdate
	render
	componentDidUpdate
	
任何时候，父组件都可以改变子组件的props，这时就会调用componentWillReceiveProps钩子方法。

调用shouldComponentUpdate在组件渲染时进行精确优化。如果你确定组件或者子组件不需要渲染新的props或者state时，该方法返回false。（首次渲染或者强制调用forceUpdate方法后，这个方法不会被调用）。false说明不调用render方法以及componentWillUpdate和componentDidUpdate方法。

使用了immutable（不可变）数据结构作为state, 同时在render方法中读取state和props，那可以重写shouldComponentUpdate,用来比较新旧props和state

另外一个性能调优使用react插件提供的PureRenderMixin方法

新的props和state会触发componentWillUpdate方法。此方法中不能再更新props和state了。应该在componentWillReceiveProps更新state

	var SimpleDiv = React.createClass({
		getDefaultProps: function() {
			return {
				colorIndex: -1
			};
		},
		
		getInitialState: function(){
			return {
				backgroundColor: 'blue',
				height: 200,
				width: 200
			};
		},
		
		update: function(){
			this.setProps({colorIndex: this.props.colorIndex + 1});
		},
		
		componentWillReceiveProps: function(props){
			var color = this.props.colors.split(',')[props.colorIndex];
			if(!color)
			  this.setProps({colorIndex: 0});
			this.setState({backgroundColor: color});
		},
	
		render: function() {
			return (<div style={this.state} onClick={this.update}/>);
		}
	});
	
	React.render(<SimpleDiv colors="Red, Green, Gray"/>, document.body);



- 销毁&清理期(Unmounting)

hook方法有

	componentWillUnmount
	
为了防止内存泄漏，此方法需要将componentDidMount中添加的任务撤销，例如定时器，闭包引用，事件侦听期。如果把父组件unmount了，子组件也会被unmount。 下面是手动unmount组件。

	element.onclick = function(){
		React.unmountComponentAtNode(document.getElementById('container'));
		alert('component has been unmounted.')
	}


## Anti-Pattern

不要在getInitialState中通过this.props来创建state是一个反模式(anti-pattern)

	getDefaultProps: function(){
		return {
			date: new Date()
		}
	},
	
	getInitialState: function(){
		return {
			day: this.props.date.getDay()
		}
	},
	
	render : function(){
		return <div>Day: {this.state.day} </div>;
	}
	
应该计算props值，保证能够同步派生的props值。

	getDefaultProps: function(){
		return {
			date: new Date()
		}
	},
	
	render: function(){
		var day = this.props.date.getDay();
		return <div>Day: {day}</div>;
	}


如果不是同步，只是初始化state，那么可以在getInitialState中操作props

##数据流

如果父组件的props改变了，react会递归向下遍历组件树，重新渲染所有使用这个属性的组件。可以简单地把组件看成一个函数，接受props和state参数并返回一个虚拟DOM

使用props把任意类型的数据传递给组件。

在使用组件的时候设置

		var surveys = [{title: "问卷调查"}];
		<ListSurvay surveys={surveys} />
		
使用setProps,但是很少用到, 只能在子组件或者组件外调用，不能this.setProps调用，在组件内可以使用state.

	var surveys = [{title: "问卷调查"}];
	var listSurveys = React.render(
		<ListSurvey />,
		document.getElementById("container")
	);
	listSurveys.setProps({surveys: surveys});
	
访问使用this.props，但是不能用这种方式设置。

注入javascript

	<a href={'/surveys/' + survey.id}>{survey.title}</a>
	
把prop设置成一个对象

	render: function(){
		var props = {
			name: 'foobar',
			age: 30
		};
		return <SurveyTable surveys={props} />
	}
	
props设置成事件处理器

	handleClick: function(){
		//...
	}

	render: function(){
		return (
			<a className='btn save' onClick={this.handleClick}>Save</a>
		)
	}
	
PropTypes在组件中定义一个配置对象，验证props,如果传递的属性与propTypes不符合将会打印console.warn日志。propTypes不是强制性的，但是可以很好的描述组件的API.

	var SurveyTableRow = React.createClass({
	
	  propTypes: {  
	  	surveys: React.PropType.shape({
	  		id: React.PropTypes.number.isRequired
	  	}).isRequired,
	  	
	  	onClick: React.PropTypes.func
	  }
	  
	});
	
使用getDefaultProps针对非必需属性设置默认值, 这个是在React.createClass时调用，而不是在实例化组件时调用。返回值被缓存起来，所以不能在这里使用实例化的数据。

	getDefaultProps: function(){
		return {
			surveys: []
		};
	}
		
		
## State

每个组件都有自己的state, state与props的区别是，state只存在于组件内部。用state确定视图的状态。

	var CountryDropdown = React.createClass({
		getInitialState: function(){
			return {
				showOptions: false
			}
		},
		
		render: function(){
			var options;
			
			if(this.state.showOptions) {
				options = (
					<ul className='options'>
					  <li>USA</li>
					  <li>China</li>
					  <li>JP</li>
					</ul>
				)
			}
			
			return (
				<div className='dropDown' onClick={this.handleClick}>
				  <label>选择国家</label>
				</div>
			)
		},
		
		handleClick: function(){
			this.setState({
				showOptions: true
			})
		}
	});
	
通过setState设置state, 也可以通过getInitialState设置默认state,只要setState被调用，render就会被调用，如果render的返回值有变化，虚拟DOM就会更新，真实的DOM就会被更新。只能通过this.setState或者replaceState方法修改状态。状态针对不同的组件独立开发，应用会变得容易调试。如果使用不可变数据结构，表示状态时，可以使用replaceState方法替换原来的状态。setState只是合并状态。replaceState必须替换的state对象和原来一样，否则可能会导致render方法执行错误，因为有可能会有属性被丢弃。

注意

- 不要在state中保存计算值，保存简单值
- 不要把props赋值给state,把props尽可能当作数据源。
- state的值是merge得到的

##事件处理

- React有自己的事件抽象。

	- 普通化事件行为,解决不同浏览器事件的差异
	- 统一了DOM事件(onChange...)和组件事件(领域事件)

- SyntheticEvent


虽然虚拟DOM上绑定了事件处理onClick,但是React的内部实现和HTML的内部实现是不同的。
https://facebook.github.io/react/docs/event.html

- 触控事件需要调用React.initializeTouchEvents(true);

		onTouchStart
		onTouchEnd
		onTouchMove
		onTouchCancel 


获取事件对象值，可以通过event.target.value。React的事件对象是SyntheticEvent实例,表现和功能上与浏览器事件相同，但是它消除了浏览器之间的兼容性。可以通过SyntheticEvent的nativeEvent属性获取浏览器原生事件对象。

- 组件事件

领域特定事件，更高层次的抽象(onInterval)

	var Timer = React.createClass({
	
		propTypes: {
			onInterval: React.PropTypes.func.isRequired,
			interval: React.PropTypes.number.isRequired
		},
		
		render: function(){
			return <div style={{display: 'none'}} />;
		},
		
		componentDidMount: function(){
			setInterval(this.props.onInterval, this.props.interval);
		}
	});
	
	function tick() {console.log('tick...')}
	
	React.render(<Timer onInterval={tick} interval={1000} />, document.body)


##组件组合

React组件是不可以扩展，继承的，而是通过组件之间的组合来构建应用程序。

分析应用，从最小，最底层的组件开始创建，然后再组合。

子组件和父组件通信的方式使用props,父组件通过属性留一个回调函数，子组件在需要时调用。

	getInitialState: function(){
		return {
			notes:  [
				'看书',
				'写代码',
				'睡觉'
			]
		};
	},

	render: function(){
		return (
				<div className="board">
					{this.state.notes.map(function(note, index){
						return (
								<Note key={index}>{note}</Note>
							)
					})}
				</div>
			)
	}


## Ref

使用referrence存储数据,允许父组件在render方法之外保持对子组件的一个引用。this.refs.newTxt是一个支持实例。它不是真实的DOM, 通过getDOMNode()能获取真实的DOM.

只有在其他常规解决方案无法使用时，才考虑使用ref和getDOMNode()方法，使用它们会阻碍React性能优化，并且会增加应用的复杂性。

	save: function(){
		var noteContent = this.refs.newTxt.getDOMNode().value; //获取真实DOM的值
		alert('保存: ' + noteContent);
		this.setState({editing: false});
	},
	renderEditing: function() {
    	return (
    		<div className="note">
    		<textarea ref="newTxt"
    		          defaultValue={this.props.children}
    		          className="form-control">
    		</textarea>
    		<button onClick={this.save}
    		        className="btn btn-success btn-sm glyphicon glyphicon-floppy-disk" />
    		</div>
    	)
	},
	
## propTypes

帮助react验证property

	propTypes: {
		count: function(props, propName) {
			alert(props)

			if(typeof props[propName] !== 'number') {
				return new Error('count属性必须是数字');
			}

			if(props[propName] > 100) {
				return new Error('todo事项不能超过100个');
			}
		}
	},

另外一个实例:
	
	propTypes: {
      name: React.PropTypes.string.isRequired,
      count: function(props, propName, componentName) {
          if(props[propName] < 10) {
              alert(111)
          }
      }
    },
	
##Props

需要动态计算的，使用{}, 静态文本的使用"", props可以是一个event handler

	<div className={this.props.dynamicClassName} />
	<div className="static-class-name" />
	<div className="edit" onClick={this.edit}>
	  Edit
	</div>
	
props可以包含children

	<p className="maybe-has-children">
		{this.props.children && React.Children.count(this.props.children) ? this.props.children : <a onClick={this.createChidren}>Make new child</a>}
	</p>

内嵌的{}可以是任何表达式

	<BandRoster>
		<Member name="foobar" />
		{[<Member name="chachacha" />, <Member name="dadad" />]}		Hello World
		{" Come on"}
		{!this.valid? && <Member name="hehe" />}		{/* comment*/}	 
	</BandRoster>

通过props进行更新删除

	note.js
	---
	save: function(){
		//触发onChange,子组件向父组件传递数据
		this.props.onChange(this.refs.newTxt.getDOMNode().value, this.props.index);
		this.setState({editing: false});
	},

	remove: function(){
		this.props.onRemove(this.props.index);
		this.setState({editing: false});
	},
	
	board.js
	---
	update: function(newTxt, index) {
		var arr = this.state.notes;
		arr[index] = newTxt;
		this.setState({notes: arr});
	},

	remove: function(index){
		var arr = this.state.notes;
		arr.splice(index, 1);
		this.setState({notes: arr});
	},

	//将render中map部分抽离出来，尽量保持render的简单性
	eachNote: function(note, index){
		return (
				<Note key={index}
							index={index}
							onChange={this.update}
							onRemove={this.remove}
				>{note}</Note>
			)
	},

	render: function(){
		return (
				<div className="board">
					{this.state.notes.map(this.eachNote)}
				</div>
			)
	}
	
	
## Key

## 一般开发工具

- npm
- package.json
- CommonJS
- httpster
- react-tools

## Mixin

将一些常用的代码合并到许多component中。

mixin就是混合进组件中的对象，能够防止静默函数覆盖，同时支持多个mixin混合。如果mixin中的方法和组件类中的方法存在重复键，React会给出警告提示。mixin就是合并。

component开头的生命周期方法，会按照在mixin数组中定义的顺序被调用。然后再调用组件类中的方法。

	var Highlight = {
		componentDidUpdate: function(){
			var node = $(this.getDOMNode());
			node.slideUp();
			node.slideDown();
		}
	};
	
	var Counter = React.createClass({
		mixins: [Highlight]
	})


##JSX Tools

以下是两种不同的option, -w是一直开启，如果jsx模版有变化，那么jsx tools就会编译。

jsx -x jsx . .

jsx -w -x jsx . .


## Youtube参考视频

- Immutable Data in React & Flux by LEE BYRON @leeb

  经常


	http://facebook.github.io/immutable-js/
	
  webpack版本
  
    	import {list} from "immutable";
    	var list = List.of(1,2,3);
    	var list2 = list.push(4);
    	list //[1,2,3]
    	list2 //[1,2,3,4]
    	
    	List.prototype.push = function(value) {
    	  var clone = deepCopy(this);
    	  clone[clone.length] = value;
    	  return clone;
    	}
    	
    	
    	import  {Map} from "immutable"    	    
    	var first = Map({key: 'value'});
    	var second = first.set('foo', "bar");
    	second === first; //false
    	
    	var third = first.set('key', 'value');	
    	third === first //true
    	
    	
    	//deep
    	import {Map} from 'immutable'
    	
    	var first = Map({
    	  foo: Map({
    	    val: 10
    	  }),
    	  
    	  bar: Map({
    	    val: 20
    	  })
    	});
    	
    	var second = first.setIn(["foo", "val"], 100);
    	second === first //false
    	second.get("foo") === first.get("foo") //false
    	second.get("bar") === first.get("bar") //true 叶子节点bar的结构没有变化，公用的 
    
 
 retrieval(trie)算法节省内存。


 缓存(Memoization)
 
 希望达到这样的效果
 
 
       function rawSum(list) {
         return list.reduce((a, b) => a + b);
       }
       
       var sum = memoize(rawSum);
       var array = [1,2,3,... 1000000];
       
       sum(array);             //500000500000
        
       sum(array);             //500000500000
       
       array.push(1000001);
       
       sum(array);             // 50000015000001
 
       
       //老版本
       
       function memoize(fn) {
         var cache = {};
         return function(arg) {
         	var hash = arg === object(arg) ?
         	    JSON.stringify(arg) : currentArg; //JSON.stringify消耗内存
         	return hash in cache ? cache[hash] : (cache[hash] = fn.call(this,arg));
         }
       }
       
       //新版本
       
       function memoize(fn) {
         var prevArg;
         var preResult;
         
         return function(arg) {
           return arg === prevArg ? 
                  prevResult :
                  (
                    prevArg = arg,
                    prevResult = fn.call(this.arg)
                  );
         }
       }
       
       
              
       var sum = memoize(rawSum);
       var array = [1,2,3,... 1000000];
      
       console.time("sum");
       sum(array);             //500000500000
       console.timeEnd("sum"); //sum: 59.251ms 
        
       console.time("sum")
       sum(array);             //500000500000
       console.timeEnd("sum")  // 0.007ms (缓存后的效果)
       
       array.push(1000001);
       
       console.time("sum")
       sum(array);             // 应该是50000015000001，实际是500000500000, 原因是arg === prevArg,发现还是同一个对象(array),只是其中内容变了
       
       console.timeEnd("sum")  // 0.006ms (缓存后的效果)
       
       
       //使用immutable
       
       import { List } from "immutable"
       
       var sum = memoize(rawSum);
       var array = [1,2,3,... 1000000];
      
       console.time("sum");
       sum(array);             //500000500000
       console.timeEnd("sum"); //sum: 59.251ms 
        
       console.time("sum")
       sum(array);             //500000500000
       console.timeEnd("sum")  // 0.007ms (缓存后的效果)
       
       array = array.push(1000001); //array是一个不同对象
       
       console.time("sum")
       sum(array);             // 应该是50000015000001
       console.timeEnd("sum")  // 76.265ms
       
       
       
React shouldComponentUpdate(nextProps, nextState)

		shouldComponentUpdate(nextProps, nextState) {
			//default
			return true; //javascript object is mutable
		}
		
		shouldComponentUpdate(nextProps, nextState) {
			return !shallowEqual(this.props, nextProps) || !shallowEqual(this.states, nextState)
		}
		
		function shallowEqual(prevObj, nextObj) {
			for(var key in prevObj) {
				if( !(key in nextObj) || prevObj[key] !== nextObj[key]) { //prevObj[key] !== nextObj[key]使用immutable
					return false;
				}
			}
			
			for(var key in nextObj) {
				if(! (key in prevObj)) {
					return false;
				}
			}
			
			return true;
		}

		||===>
		
		mixinx: [React.addones.PureRenderMixin]
		
		
Immutable Data is faster, and less memory

