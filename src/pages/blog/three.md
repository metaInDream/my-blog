# Threejs

## 基础

### Object3D

- expandByObject
- setFromObject

### geometry

- computeBoundingBox()：调用后，boundingBox 属性才有值，返回 Box3
- computeBoundingSphere():调用后，boundingSphere 属性才有值，返回Sphere
- center(): 几何体居中

### Box

包围盒是一种计算一系列顶点最优包围空间的算法。

封装与包围盒算法相关的三个API分别是：Box3（三维包围盒）、Sphere（包围球）、Box2（包围矩形）。

#### Box2
表示一个矩形区域，属性值是二维向量对象Vector2，通过Box2的构造函数可以直接设置min和max属性值，也可以通过Box2的一些方法来设置。
- min属性值是Vector2(Xmin, Ymin)，也可以直接设置
- max属性值是Vector2(Xmax, Ymax)，也可以直接设置
```
var box2 = new THREE.Box2(new THREE.Vector2(0, 0), new THREE.Vector2(25, 25))
box2.min = new THREE.Vector2(0, 0);
box2.max = new THREE.Vector2(25, 25);
```
#### Sphere
一个球形的包围区域，通过球心坐标.center和半径.radius两个属性来描述.

```
var sphere = new THREE.Sphere()
sphere.center = new THREE.Vector3(-10, -10,0); // 设置球心坐标
sphere.radius=20; // 设置包围球半径
```

#### Box3
表示三维长方体包围区域，使用min和max两个属性来描述该包围区域，Box3的min和max属性值都是三维向量对象Vector3。
描述一个长方体包围盒需要通过xyz坐标来表示，X范围[Xmin,Xmax],Y范围[Ymin,Ymax],Z范围[Zmin,Zmax]

- min属性值是Vector3(Xmin, Ymin, Zmin)
- max属性值是Vector3(Xmax, Ymax, Zmin)
- getSize(Vector3)：长宽高尺寸
- getCenter(Vector3)：几何中心
- setFromPoints(points: Vector3[]) 设置此包围盒的上边界和下边界，以包含数组 points 中的所有点。
- expandByObject()：层级模型的包围盒
- expandByScalar(scalar)：包围盒整体尺寸放大，+scalar
- getBoundingSphere()：包围盒Box3和包围球Sphere可以相互等价转化，通过包围盒对象来计算包围球对象，注意：半径是中心到一个顶点的距离

```
var box3 = new THREE.Box3()
box3.min = new THREE.Vector3(-10, -10,0);
box3.max = new THREE.Vector3(100, 20,50);
```

### clone & copy

- .clone()是相当于新建一个对象，然后复制原对象的属性值赋值给新的对象对应属性，创建一个和原来对象完全一样的对象。
- .copy()方法简单的说就是复制一个对象的属性值赋值给给另一个对象对应的属性。

几何体和Vector3表现一样

```
var box=new THREE.BoxGeometry(10,10,10);
var box2 = box.clone();//克隆几何体
box.translate(20,0,0);//平移原几何体  新克隆的几何体不受影响
var material=new THREE.MeshLambertMaterial({color:0x0000ff});//材质对象—蓝色
var material2=new THREE.MeshLambertMaterial({color:0xff0000});//材质对象—红色
var mesh=new THREE.Mesh(box,material);
var mesh2=new THREE.Mesh(box2,material2);
```

```
var box=new THREE.BoxGeometry(10,10,10);//创建一个立方体几何对象
var sphere=new THREE.SphereGeometry(10,40,40);//创建一个球体几何对象
box.copy(sphere);//球体数据复制到box几何体，替换box原来的顶点数据
```

Mesh（网格模型）：clone时，模型的几何体（Geometry）和材质(Material)对象是共享的。改变会影响所有的网格模型


## 场景

### 自旋转

#### 平移

方法：
    - 查物体中心
    - 通过translate移动到原点，旋转
    - 在移回

#### 物体中心自旋转
方案一：创建一个对象，把物体加入该对象中，获取物体的中心（center），并将3D对象的位置(position)设置为中心，物体的位置（position）反向处理

```
let box = new THREE.Box3();
box3.expandByObject(group);
let center = box3.getCenter(new THREE.Vector3());
let wrapper = new THREE.Group();
wrapper.position.copy(center);
group.postion.set(-center.x, -center.y, -center.z);
wrapper.add(group)

------------------------------------------------------
```



### 模型居中

两种方法：

- 建模师处理模型，将模型的位置移动到（0， 0， 0）处即可
- 代码加载处理，分两种情况
    - 单个网络模型：如果加载或生成的模型对象只有一个网格模型，不是多个模型对象组成的层级模型，可以通过控制网格模型模型几何体的方式居中。
    - 多个模型对象：涉及到模型中心坐标 center 和 模型位置 position的运算，通过移动position的位置，把center变成原点，position = -(center - postion)

单模型

```
Mesh.geometry.center()
```

> 如不居中时，查看lookAt() 是否指向坐标原点 和 position 坐标是否（0,0,0）

多模型

```
let box = new THREE.Box3();
box.expandByObject(xxxmodel);
let center = box.getCenter(new THREE.Vector3()); // 中心点
let size = box.getSize(new THREE.Vector3());  // 长宽高
let { postion } = xxxmodel; // 位置坐标
xxxmodel.position.x = position.x - center.x
xxxmodel.position.y = position.y - center.y
xxxmodel.position.z = position.z - center.z
```


## 项目

## 资源

- [全景图片](http://www.humus.name/index.php?page=Textures)

## 参考资料






