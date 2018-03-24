## 作业内容 
### 1、简答题 
---
    1.解释 游戏对象（GameObjects） 和 资源（Assets）的区别与联系。  

- 对象：直接出现在游戏的场景中，是资源整合的具体表现，对象一般有玩家、敌人、环境、摄像机和音乐等虚拟父类，这些父类本身没有实体，但他们的子类包含了游戏中会出现的对象。 
- 资源：资源可以被一个或多个对象使用。有些资源作为模板，可实例化成游戏中具体的对象。资源文件夹通常有对象、材质、场景、声音、预设、贴图、脚本、动作，在这些文件夹下可以继续划分。 
---    
    2.下载几个游戏案例，分别总结资源、对象组织的结构（指资源的目录组织结构与游戏对象树的层次结构）  
![u=3375423087,1211204506&fm=27&gp=0]($res/u=3375423087,1211204506&fm=27&gp=0.jpg)

    答：我下载的是《双刃战士》这款小游戏,其中资源目录组织结构如下：
![BEFQBT2ZT0Z[D8S9]HYP45U]($res/BEFQBT2ZT0Z%5BD8S9%5DHYP45U.png)

---
   
    3.编写一个代码，使用 debug 语句来验证 MonoBehaviour 基本行为或事件触发的条件
```
using System.Collections;  
using System.Collections.Generic;  
using UnityEngine;  
  
public class Demo : MonoBehaviour {  
    void Awake() {  
        Debug.Log ("In Awake!");  
    }  
  
    // Use this for initialization  
    void Start () {  
        Debug.Log ("In Awake!");  
    }  
      
    // Update is called once per frame  
    void Update () {  
        Debug.Log ("In Update!");  
    }  
  
    void FixedUpdate() {  
        Debug.Log ("In FixedUpdate!");  
    }  
  
    void LateUpdate() {  
        Debug.Log ("In LateUpdate!");  
    }  
  
    void OnGUI() {  
        Debug.Log ("In OnGUI!");  
    }  
  
    void OnDisable() {  
        Debug.Log ("In OnDisable!");  
    }  
  
    void OnEnable() {  
        Debug.Log ("In OnEnable!");  
    }  
} 
```
---
	
    4.查找脚本手册，了解 GameObject，Transform，Component 对象
* 分别翻译官方对三个对象的描述（Description） 
   * 游戏对象是Unity中表示游戏角色，游戏道具和游戏场景的基本对象。他们自身无法完成许多功能，但是他们构成了那些给予他们实体功能的组件的容器。
   * 转换组件决定了游戏场景中每个游戏对象的位置，旋转和缩放比例。每个游戏对象都有转换组件。
   * 组件是游戏中对象和行为的细节。它是每个游戏对象的功能部分。
* 描述下图中 table 对象（实体）的属性、table 的 Transform 的属性、 table 的部件
  答：
    Tag属性用于区分游戏中不同类型的对象，Tag可以理解为一类元素的标记，可用GameObject.FindWithTag()来查询对象。GameObject.FindWithTag()只返回一个对象，如果想获取多个Tag为某值的对象，应该用GameObject.FindGameObjectsWithTag()。第一个选择框是activeSelf属性。Layer属性默认是Default。Table的Transform属性：Position、Rotation、Scale     Table的部件：Mesh Filter、Box Collider、Mesh Renderer						 
---
* 用 UML 图描述 三者的关系（请使用 UMLet 14.1.1 stand-alone版本出图）

![]($res/QQ%E5%9B%BE%E7%89%8720180324194143.png)

---
整理相关学习资料，编写简单代码验证以下技术的实现：
 *   查找对象 	
```
public static GameObject Find(string name)
```
 *   添加子对象
```
public static GameObect CreatePrimitive(PrimitiveTypetype)
```
 
*   遍历对象树
```css
foreach (Transform child in transform) {
    Debug.Log(child.gameObject.name);
}
```
*   清除所有子对象
```css
 foreach (Transform child in transform) {
     Destroy(child.gameObject);
 }
```
---
资源预设（Prefabs）与 对象克隆 (clone)

*   预设（Prefabs）有什么好处？

    答：预设资源储存了完整储存了对象的组件和属性，相当于模板，方便重复使用。预设与对象克隆不同的是，预设与实例化的对象有关联，而对象克隆本体和克隆出的对象是不相影响的。

*   预设与对象克隆 (clone or copy or Instantiate of Unity Object) 关系？

    答：对象克隆不受克隆本体的影响，因此A对象克隆的对象B不会因为A的改变而相应改变。
*   制作 table 预制，写一段代码将 table 预制资源实例化成游戏对象

```css
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
public class Demo: MonoBehaviour {
    public GameObject MyTable; 
	// Use this for initialization  
    void Start () {
        GameObject instance=(GameObject)Instantiate(MyTable, transform.position, 												transform.rotation);
    } 
	// Update is called once per frame  
    void Update () {}
}
```
---
* 尝试解释组合模式（Composite Pattern / 一种设计模式）。
	
	答：组合模式允许用户将对象组合成树形结构来表现”部分-整体“的层次结构，使得客户以一致的         方式处理单个对象以及对象的组合。组合模式实现的最关键的地方是——简单对象和复合对象必须实现相同的    接口。这就是组合模式能够将组合对象和简单对象进行一致处理的原因。

  *   使用 BroadcastMessage() 方法向子对象发送消息
	
      子类对象方法：
         ```
            void sendMessage() {  
                 print("Hello World!");  
            }  
         ```
  	  父类对象方法：         
         ```
            void Start () {  
                 this.BroadcastMessage("sendMessage");  
            }  
         ```

### 2、 编程实践，井字棋小游戏
     
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class 井字棋1 : MonoBehaviour {

    private int turn = 1;
    private int[,] state = new int[3, 3];
    public Texture2D img;
    public Texture2D img1;
    public Texture2D img2;
    // Use this for initialization
    void Start () {
        Reset();
    }
    void Reset()
    {
        turn = 1;
        for (int i = 0; i < 3; ++i)
        {
            for (int j = 0; j < 3; ++j)
            {
                state[i, j] = 0;
            }
        }
    }
    
    void OnGUI()
    {
        GUIStyle fontStyle = new GUIStyle();
        GUIStyle fontStyle1 = new GUIStyle();
        GUIStyle fontStyle2 = new GUIStyle();
        fontStyle.fontSize = 30;
        fontStyle1.normal.background = img;
        fontStyle2.fontSize = 25;
        fontStyle2.normal.textColor = new Color(255, 255, 255);

        GUI.Label(new Rect(0, 0, 1024, 781), "", fontStyle1);
        GUI.Label(new Rect(260, 30, 100, 100), "介四里没有挽过的船新井字棋", fontStyle);
        GUI.Label(new Rect(140, 60, 100, 100), "挤需体验三番钟,里造会干我一样,爱象节款游戏!", fontStyle);
        GUI.Label(new Rect(140, 150, 150, 100), img1);
        GUI.Label(new Rect(690, 150, 150, 100), img2);

        if (GUI.Button(new Rect(415, 340, 80, 50), "重置"))
            Reset();
        int result = check();
        if (result == 1)
        {
            GUI.Label(new Rect(140, 100, 160, 50), "O 赢!", fontStyle2);
        }
        else if (result == 2)
        {
            GUI.Label(new Rect(690, 100, 160, 50), "X 赢!", fontStyle2);
        }

        for (int i = 0; i < 3; ++i)
        {
            for (int j = 0; j < 3; ++j)
            {
                if (state[i, j] == 1)
                    GUI.Button(new Rect(335 + i * 80, 90 + j * 80, 80, 80), img1);
                if (state[i, j] == 2)
                    GUI.Button(new Rect(335 + i * 80, 90 + j * 80, 80, 80), img2);
                if (GUI.Button(new Rect(335 + i * 80, 90 + j * 80, 80, 80), ""))
                {
                    if (result == 0)
                    {
                        if (turn == 1)
                            state[i, j] = 1;
                        else
                            state[i, j] = 2;
                        turn = -turn;
                    }
                }
            }
        }
    }
    int check()
    {
        // 横向连线    
        for (int i = 0; i < 3; ++i)
        {
            if (state[i, 0] != 0 && state[i, 0] == state[i, 1] && state[i, 1] == state[i, 2])
            {
                return state[i, 0];
            }
        }
        //纵向连线    
        for (int j = 0; j < 3; ++j)
        {
            if (state[0, j] != 0 && state[0, j] == state[1, j] && state[1, j] == state[2, j])
            {
                return state[0, j];
            }
        }
        //斜向连线    
        if (state[1, 1] != 0 &&
            state[0, 0] == state[1, 1] && state[1, 1] == state[2, 2] ||
            state[0, 2] == state[1, 1] && state[1, 1] == state[2, 0])
        {
            return state[1, 1];
        }
        return 0;
    }
}

```
#### 这里借鉴了添加背景图片和素材的方法，最终效果如下：




![]($res/QQ%E5%9B%BE%E7%89%8720180324205610.png)

![]($res/QQ%E5%9B%BE%E7%89%8720180324205832.png)

![]($res/QQ%E5%9B%BE%E7%89%8720180324205951.png)




    