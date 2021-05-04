# Mongoose框架

> mongoose框架是一个数据库访问框架，以面向对象的方式来操作数据库，开发者通过Mongoose框架方法，创建ODM(对象文档模型)，实现对象与文档的相互转换。
>
>  
>
> MongoDB与关系型数据库名称对应说明：
>
> 1. 数据库 ---> 关系型数据库的数据库
> 2. 集合 ---> 关系型数据库中的表
> 3. 文档 ---> 表中的一行数据
> 4. 字段 ---> 表中的列

## 使用Mongoose框架

> 步骤：
>
> 1. 打开终端在任意路径下，执行`mongod --dbpath mongodb保存所有数据库的路径`，启动mongoDB服务。
> 2. 在项目根目录下，执行`yarn add mongoose`命令，下载mongoose包。
> 3. 使用CommonJS引入mongoose包。
> 4. 设置mongoose连接数据库的路径和一些其它配置信息。
> 5. 添加open监听，并连接mongoDB。

```javascript
let mongoose = require('mongoose');
// 设置连接路径+配置信息
mongoose.connect('mongodb://localhost:27017/demo',{
    useNewUrlParser: true, // 3.1.x版本以上需要添加该属性，使用新的url解析器。
    useUnifiedTopology: true, // 让mongodb使用新的服务发现和监控引擎，旧版弃用。
    useCreateIndex: true // 索引弃用方法，系统默认使用collection.ensureIndex()，开启该选项，让系统使用新的索引方法createIndexes()。
});

// 连接demo数据库
mongoose.connection.on('open', (err)=>{
  if(err){
    console.log('数据库连接失败');
    return;
  }
  console.log('数据库连接成功')
});
```



## 创建ODM对象并执行CRUD操作

> 创建ODM对象(对象文档模型)：
>
> 1. 获取模式构造函数对象，`mongoose.Schema`。
> 2. 根据模式构造函数对象创建集合(相当于关系型数据库中的表)中字段约束。
> 3. 根据约束对象创建ODM对象。
>
> CRUD操作：
>
> 1. Create(插入)：create(object, callback(err,data));
> 2. retrieve(查询): 
>    1. find({查询条件}，{投影(显示字段)}，callback(err, data)). --> 遍历整个集合
>    2. findOne({查询条件}，{投影(显示字段)}，callback(err, data))  --> 遍历集合只要找到一个就不在向下遍历。
> 3. update(更新):
> 	 1. updateMany({查询条件}，{修改字段:数据}，callback(err, data)). --> 遍历整个集合
>    2. updateOne({查询条件}，{修改字段:数据}，callback(err, data))  --> 遍历集合只要找到一个就不在向下遍历。
> 4. delete(删除):
>    1. deleteOne({查询条件}，callback(err，data)) --> 遍历集合只要找到一个就不在向下遍历。
>    2. deleteMany({查询条件}，callback(err，data)) --> 遍历整个集合，删除符合条件的文档。

```javascript
let mongoose = require('mongoose');
// 省略连接配置
mongoose.connection.on('open', (err)=>{
  if(err){
    console.log('数据库连接失败');
    return;
  }
  console.log('数据库连接成功')
  // 1. 获取模式构造函数对象
	let SchemaObj = mongoose.Schema;
	// 2. 根据模式构造函数对象创建集合的约束对象
	let studentRule = new SchemaObj({
	  stu_id:{ // stu_id有多个约束时，使用对象记录
    	type: String, // 类型为字符串
    	required: true, // 必填项
  	  unique: true // 必须唯一
	  },
  	name:{
	    type: String,
    	required: true
  	},
	  age: Number, // 只有一个约束，类型为数字
  	address: String,
	  hobby: [String], // 只有一个约束，类型为字符串数组
  	info: SchemaObj.Types.Mixed, // 只有一个约束，该字段支持任何类型
	  date: {
    	type: Date,
  	  default: Date.now() // 字段没有设置值时，默认值
	  },
  	enable_flag: {
	  	type: Boolean,
    	default: true // 字段没有设置值时，默认值
		}
	});
  
	// 3. 根据约束对象创建ODM，mongoose.model('集合名称'，集合约束)
	let studentModel = mongoose.model('student', studentRule);
  
  insertModel(studentModel);  // 插入数据
  retrieveModel(studentModel); // 查询数据
  updateModel(studentModel); // 更新数据
  deleteModel(studentModel); // 删除数据
});


// 下面是CRUD操作
function insertModel(stuModel){
  	setTimeout(()=>{
    // 使用create方法将student对象，保存到数据库中，通过回调函数获取操作结果。
    stuModel.create({
        stu_id:'10002',
        name:'李四',
        age: 18,
        address:'黄冈市花溪小区',
        hobby:['看书','跳舞'],
        info:'一个谜一样的男子'
    },err1 => {
        console.log(err);
    });
  },100);
}

function retrieveModel(stuModel){
   setTimeout(()=>{
     // 使用find方法查询数据，通过回调函数获取操作结果。
    	stuModel.find({stu_id:'10001'},{_id:0, __v:0},(err, data)=>{
      	  if (err){
      	      console.log(err);
      	      return;
     		   }
      	   console.log(data);
       });
     
      // 使用findOne方法查询数据，通过回调函数获取操作结果。
    	stuModel.findOne({age:18},{stu_id:1, name:1, age:1, _id:0},(err, result) => {
      	  if (err){
        	    console.log(err);
          	  return;
        	}
	        console.log('findOne',result);
  	  });
   },100)
}

function updateModel(stuModel){
    setTimeout(()=>{
      // 更新一个
      stuModel.updateOne({name:'张三'},{age:2, address:'火星区'},(err,result) => {
        if (err){
            console.log(err);
            return;
        }
        console.log('findOne',result);
    });

     // 全部更新
    stuModel.updateMany({age:18},{info:'你到底是谁'}, (err, result)=>{
        if (err){
            console.log(err);
            return;
        }
        console.log('findOne',result);
    });
    },100);
}

function deleteModel(stuModel){
    setTimeout(()=>{
      	// 删除一个
  	    stuModel.deleteOne({stu:'10001',name:'lilei'}, (err, result) =>{
	        if (err){
            	console.log(err);
          	  return;
        	}
      	  console.log('findOne',result);
    	});
      
			// 删除多个
  	  stuModel.deleteMany({age:18}, (err, result)=>{
	        if (err){
            	console.log(err);
          	  return;
        	}
      	  console.log('findOne',result);
    	});
    },100);
}
```



### ODM对象注意

> 运行下面代码有两种情况：
>
> 1. 如果数据库中没有`student`集合时，MongoDB会自动根据约束对象创建该集合。
> 2. 如果数据库中已经有`student`集合时，MongoDB会忽略新设置的约束对象，也就是说使用`mongoose.model()`函数，不会重新设置集合的约束，集合还是以第一次设置的约束为准。
>
>  
>
> 当插入数据时，不给字段设置数据时，MongoDB会自动添加一个空字符串。

```javascript
let mongoose = require('mongoose');
// 省略连接配置
mongoose.connection.on('open', (err)=>{
  if(err){
    console.log('数据库连接失败');
    return;
  }
  console.log('数据库连接成功')
  // 1. 获取模式构造函数对象
	let SchemaObj = mongoose.Schema;
	// 2. 根据模式构造函数对象创建集合的约束对象
	let studentRule = new SchemaObj({....});
  let stuModel = mongoose.model('student', studentRule);
});
```



### 只读ODM对象

> 上面创建的ODM对象，可以实现对数据库的读写操作，如果有些业务需求只需要读取，无需写入时，就可以使用只读ODM对象，例如读取保存省市县的数据库。`只读ODM对象其实就是创建时不设置任何规则`。

```javascript
let mongoose = require('mongoose');
// 省略连接配置
mongoose.connection.on('open', (err)=>{
  if(err){
    console.log('数据库连接失败');
    return;
  }
  console.log('数据库连接成功')
  // 1. 获取模式构造函数对象
	let SchemaObj = mongoose.Schema;
	// 2. 创建一个空约束对象。
	let studentRule = new SchemaObj();
  // 3. 不设置约束时，mongoose会返回一个只读的ODM对象。
  let stuModel = mongoose.model('student', studentRule);
});
```



### CRUD使用注意

> 1. 带One的函数比不带One函数效率高(`例如findOne函数和find函数`)，因为后者需要遍历整个集合，而带One的函数只需要找到一个就行。
> 2. 由于上面操作都是`异步操作`，这样就会造成，有些操作后添加但是先执行，例如上面代码如果不用定时器间隔时间，会出现这样的执行顺序`find--> update --> create --> deletes`中，因此`必须做同步`，例如上面代码中将所有CRUD操作都放在定时器中，操作间相隔100ms。
> 3. find查询方法中显示字段设置，`1代表显示，0代表不显示`，因此只有两种显示方式：
>    1. 设置只显示的：`{stu_id:1, name:1, age:1, _id:0}`，查询出来的字段只有`stu_id`、`name`、`age`字段。
>    2. 设置不显示的：`{age:0, address:0, _id:0}`，查询出来的字段没有`age`、`address`、`_id`字段。
>
> 注意：
>
> ​		当给mongodb插入数据时，数据库会自动添加_id字段，该字段就相当于关系型数据库中表的主键，默认查询时都会显示该字段，如果不想显示将`_id`设置为0。



### 投影

> ​		在find方法中，默认数据库会将查询出来的文档的所有字段都返回，如果只需要显示一部分字段，那么就需要使用投影。`投影可以理解为sql语句中的selec～from之间设置的字段名`。

