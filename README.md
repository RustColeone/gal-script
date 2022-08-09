# gal-script Language

Language for my custom interpretor gal-game maker for Visual Studio Code？

gal脚本语言的…Visual Studio Code语法着色器？

## Features 特性

Functionality is limited but the syntax is rather simple. This is not intended for any complicated operation, infact there's not even an addition calculation

虽然gal脚本这个语言功能比较少，但这也意味着使用起来会相对简单一些，只能处理特定的功能甚至不能加减乘除

> Tip: Many popular extensions utilize animations. This is an excellent way to show off your extension! We recommend short, focused animations that are easy to follow

## Requirements 需求

Obviously you'll need the interpreter, which will likely not be released in public for a while. On top of that, you need to install it in Visual Studio Code as an extension, although not available in the marketplace. There's sadly no highlights for grammer mistakes though, I just can't figure out how to do it

毕竟是Visual Studio Code用的，所以很明显需要Visual Studio Code来运行， 不过很遗憾，我们没有错误提示，我完全不会搞这些，不如说这个能做出来都不错了

## Installing 安装方式

Since this is not in the marketplace, you need to copy this manually into the extension folder under vscode, likely

因为我没有搞进Marketplace里，所以要复制进安装目录中

> C:\Program Files\Microsoft VS Code\resources\app\extensions

Your installation path may varies, but just drag it into the extensions folder

根据安装目录大概会有不同，不过都是直接扔进这个叫做extensions的文件夹就行了

## Known Issues

I have not decided on a fix for showing the English character { " }, please use { ' } or other similar character (such as Chinese) instead.

我暂时没有处理英语的 { " } 符号，如果需要使用的话可能得找相似的符号（如汉字）或者使用 { ' } 代替

## Release Notes 记录

I added...

### 1.0.0

Initial Release

-----------------------------------------------------------------------------------------------------------

## Usage 使用

### KEYWORDS 关键词
```{TRANSLATE}``` 

Seperator for translates during pre-processing, it is possible for a line to have no translation, the interpretor simply skips that empty line when processing

会在预处理中用来分隔不同翻译，如果某一行要显示的东西是空的会直接跳过这一行

Example

>你好 {TRANSLATE} Greetings

```var::```

Access the value of a runtime variable, which must be defined with /define before using it in this line

使用一个运行时才会存在的变量所储存的数值，这个变量必须曾经被 /define 定义过

> /define beenToForest false

If this line was executed before

> /if var::beenToForest == true

will be false, since beenToForest stores a false and was compared with a true

会判定为false， 因为beenToForest（曾去过森林）储存的数据为false，不等于true

```global::```

Access the value of a global variable, which must be defined with /set at somepoint in time before using it in this line，exists across the entire game instead of this save. (For example, you can enable new actions on a new save if an action was performed in the previous runs of the game)

使用一个运行时才会存在的变量所储存的数值，这个变量必须曾经被 /set 定义过，这个全局变量的存在与这个存档是否运行过这个变量无关（比如多周目时检测某行为是否曾今达成过来允许某些行为）

Usage similar to ```var::```
与```var::```相似

> /if global::beenToForest == true

### /activePrefabComponent
If an entity contains certain component (which will be find with DFS) change its state

通过DFS找到物体中的某个特定名称的组件并改变其状态

Example
```
/activePrefabComponent ${1:name_of_entity物体名称} ${2:component_name组件名称} ${3:visibility_true_false开或关}
```


### /animation
(Holdable)

Play animation by name

根据名字播放特定动画

Example
```
/animation ${1:name_of_animation}
```
### /background
Change the background image

改变背景

Example
```
/background ${1:path_to_image}
```
### /beginChoices
When this line is reached, starts choice routine and tells the interpretor how many choice it should spawn

提前告诉interpretor我们要生成选项的数量

Example
```
/beginChoices 2
    /choice 一个选项{TRANSLATE}Some Choice
        选择后运行的选项内容{TRANSLATE}Choice Content when this choice is selected
        不会看见其他选项的内容
    /choice 第二个选项{TRANSLATE}Second Choice
        选择后运行的选项内容{TRANSLATE}Choice Content when this choice is selected
        不会看见其他选项的内容
/endChoices
```
### /character
No longer used, on second thought, don't use this, this just calls entity
呃呃呃……这个和entity一模一样，所以别用
### /choice
Indicate the scope of a certain choice, what to show on a choice and what to run after a choice is picked

需要这个来指定选项按钮上显示的内容一级被选择后运行的范围

Example
```
/beginChoices 2
    /choice 一个选项{TRANSLATE}Some Choice
        选择后运行的选项内容{TRANSLATE}Choice Content when this choice is selected
        不会看见其他选项的内容
    /choice 第二个选项{TRANSLATE}Second Choice
        选择后运行的选项内容{TRANSLATE}Choice Content when this choice is selected
        不会看见其他选项的内容
/endChoices
```
### /clear
Specify a catagory to clear

指定一个类别并清除其中的内容

Example
```
/clear ${1:catagory_code_name}
```

catagory_code_name includes 包括...
- photo（照片）
- style（姓名框与对话框字体）
- speakerName（姓名框）
- dialogue（对话框）
- background（背景）
- particles（粒子效果）
- entity（所有物体）
- profileIcon（头像）
- all（全部）
### /define
Defines a variable at run time, only accessible with in this save

存档内定义一个变量，只在这个存档中生效

Example
```
/define ${1:var_name} ${2:value}"
```
### /dummy
Skips certain lines (will not be processed) untill the line that specified dummy was defined (exclusive of dummy)

跳过一定的内容到指定的dummy（不包括）为止

Dummy is a special type of variable that was defined during the pre-processing, does not need any value

Dummy是一个特殊类型的不需要任何数值的变量，会在预处理中被记下来

Example
```
/dummy someDummy
```

### /endChoices
Required to indicate that Choice scope ends here，still needs to satisfy scope

需要这个来指定选项的范围，任然需要满足scope

Example
```
/beginChoices 2
    /choice 一个选项{TRANSLATE}Some Choice
        选择后运行的选项内容{TRANSLATE}Choice Content when this choice is selected
    /choice 第二个选项{TRANSLATE}Second Choice
        选择后运行的选项内容{TRANSLATE}Choice Content when this choice is selected
/endChoices
```
### /entity
Summon an entity with a name and path the the entity. You can either use 2, 5, or 6 parameters, no skipping, unspecified parameters would use default. When entity with same name exists, replace the old entity with the same name.

赋予名字并根据指定文件生成一个物体，可以只用2，5，或6个参数，但不能跳过特定，未指定参数采用默认数值，当存在相同名字的物体存在时，旧物体会被新物体替换

Example
```
/entity ${1:name} ${2:path_to_prefab} ${3:x} ${4:y} ${5:z} ${6:visibility_true_false}
```

Removing all entity 清除所有物体
```
/clear entity
```

### /fade
(Holdable)

On the UI layer but below the text layer, fade in or out of black

在UI层但不覆盖操控，淡入淡出黑幕

Example
```
/fade ${1:in_out} ${2:speed}
```
### /fadeEntity
Some specific entity such as the background contains a special shader that can be faded with special effects that looks like burning

特定物体（如背景）存在特殊的像是烧毁的淡出模式

Example
```
/fadeEntity ${1:name_of_entity} ${2:visibility_true_false}
```

### /hold

Waits until a certain action is complete, Or it waits for a specific period of time

等待某个行为的完成或者等待一个指定的时间

Example
```
/move someEntity 1 1 1 true 10
/hold
This will not show untill move is complete{TRANSLATE}在移动没完成之前是不会显示这段文字的
/fade false 5
/hold 1.5
Regardless of the fade's progress, wait 1.5 seconds
不管fade有没有完成，等待1.5秒
/animation slash
/hold
/hold 1.5
After the animation is complete, wait 1.5 seconds{TRANSLATE}动画完成后继续等待1.5秒
```

### /if
When a statement is true, execute the code inside this scope

当指定内容为真，运行一段代码

Example
```
/define variable 0
/if variable == 1
    This tab on the left indicate that this line is in the scope of if
    左边的这个空格（tab）意味着这段内容属于这个 if
    Runs this block of code if variable == 1 phrase to true
    如果variable == 1 为 true 才会运行这段代码
    but this will not show because variable == 0
    现在这段代码不会运行，因为 variable 等于0而不是1
Show this regardless because this is outside the scope of the if statement
这段代码不管variable是什么都会显示，因为它不属于这个if
```
### /load
Loads and starts a specific gal-script file

加载并从头读取指定gal脚本文件

Example
```
/load ${1:path_to_file}
```
### /message
Display a message with a black banner as a background， will be closed automatically when proceed to the next line

使用黑色横幅显示一段内容，进入下一行会自动关闭

Example
```
/message ${1:some_message}
```
### /modeNVL
Dialogue Text Style

显示对话的方式

Example
```
/modeNVL ${1:|true,false|}
```

### /move
(Holdable)

Move an entity specified by name

通过名字指定一个物体并移动它

Example
```
/move ${1:obj_code_name物体名称} ${2:x} ${3:y} ${4:z} ${5:is_linear是否线性移动} ${6:speed速度}
```

### /music
Play and loop music from path

从路径加载并循环播放一段音频

Example
```
/music ${1:|play,pause,stop,path_to_music|}
```

```
/music path_to_music
/music play
xxx
/music pause
```
### /notepad
Open a notepad style UI element and append text, if already opened, append only

往一个笔记本UI里面加入字符，如果没有这个UI，则打开这个UI

Can either open or close an notepad

可以打开或者关闭

Example
```
/notepad ${1:enable_true_false是否打开} ${2:message加入内容}
```

### /particles
Enable a specified particle system （Rain, Flower, Snow）

启动一个特殊的粒子效果，（如雨，花，雪）

Example
```
/particles ${1:|rain,snow,flower|} ${2:enable_true_false}
```
### /photo
Initialize a UI layer photo that can be closed by the player

打开一个UI层的照片，玩家可以自己关闭这个照片

Example
```
/photo ${1:path_to_image文件路径}
```

You can close it with
去除的方法
```
/clear photo
```
### /profileIcon
The bottom left of the screen has space for an profile-pic style icon, accessing this will set that icon to some specified sprite. If the sprite contains multiple sub-sprite, you can specify that with the subSprite parameter

屏幕的左下角能够显示一个头像，设置那个头像，如果一个图像中储存着多个独立图像（Unity sprite特有），通过指定subSprite参数可以选择剩下的其他图像

Example
```
/profileIcon ${1:path_to_image文件路径}
/profileIcon ${1:path_to_image文件路径} ${2:subSprite}
```

You can close it with
去除的方法
```
/clear profileIcon
```
### /set
Define a global variable that can be accessed across the game regardlsss of the save file

定义一个能够在任意存档中访问的全局变量

New variable with new names simply overwrites the old files

如果存在相同名字的变量，新变量会覆盖旧变量

Example
```
/set someVariable 0
```
### /setPosition
Access a entity with name, and set the position of it starting the next frame

找到一个对应名字的物体，改变它从下一帧起的位置

Example
```
/setPosition ${1:obj_code_name物体名称} ${2:x} ${3:y} ${4:z}
```
### /setScene
Skips to a new scene, **DO NOT USE**

跳转到一个新的场景，**不要用**
### /setSpeakerName
Sets the text displayed in the name text field

修改在名字栏显示的字符

Example
```
/setSpeakerName ${1:speaker_name需要显示的名字}
```
### /shake
Shake something 摇晃某个物体

Default shake camera 默认摇晃相机

Example
```
/shake
/shake ${1:obj_code_name物体名称}
/shake ${1:obj_code_name物体名称} ${2:shake_profile摇晃方式名称}
```
### /showUI
Shows UI or not, player cannot turn it back on

是否显示UI（true显示，false不显示），并且玩家不能手动重新打开

Example
```
/showUI ${1:|true, false|}
```
### /skipToAfter
Skips to a line number and is exclusive, (this is not recommended, use /skipToDummy instead)

跳到某特定行并从之后开始，（不建议使用，最好用/skipToDummy）

Example
```
/skipToAfter ${1: Line number}
```

```
1| /skipToAfter 3
2| xxx will be skipped
3| xxx 这一行会被跳过
4| xxx Only this line will be displayed {TRANSLATE} 只会显示这一行
```
### /skipToDummy
Skips certain lines (will not be processed) untill the line that specified dummy was defined (exclusive of dummy)

跳过一定的内容到指定的dummy（不包括）为止

Dummy is a special type of variable that was defined during the pre-processing, does not need any value

Dummy是一个特殊类型的不需要任何数值的变量，会在预处理中被记下来

Example
```
/skipToAfter ${1: Dummy name}
```

```
1| /skipToDummy skipToAfterThis
2| xxx This line will be skipped
3| xxx 这一行会被跳过
4| /dummy skipToAfterThis
5| xxx Only this line will be displayed {TRANSLATE}只显示这一行
```
### /sound
Plays a specific audio file at the point this line is reached, does not require any dialogue

到达这一行时播放一段音频，不需要任何对话

Example
```
/sound ${1:path_to_sound文件路径}
```
### /speechFilePath
specify an audio file for this dialogue line, needs to be appended to the end of text

想要在这段对话出现时播放的音频文件，前面必需要有一段对话

Example
```
/speechFilePath ${1:path_to_speech文件路径}

This line includes an audio file /speechFilePath ${1:path_to_speech文件路径}
```

When using with translation, the interpreter first process this command, translations will be processed later.
```
This line includes an audio file {TRANSLATE} 这行包括了一段音频文件 /speechFilePath ${1:path_to_speech文件路径}
```
### /style
Takes 2 or 6 parameters Change the style of the specified textbox (name or dialogue)

可以用两个或者六个参数，改变指定文本框(name 名 或者 dialogue 对话)的字体颜色，字体大小等
```
/style ${1:|name,dialogue|}, ${2:font_size_number}

/style ${1:|name,dialogue|}, ${1:red_float}, ${2:green_float}, ${3:blue_float}, ${3:transparancy_float}, ${6:font_size_number}
```
### /styleEntity
Takes either 4 or 5 parameters, sets the style of the entity if available (slash, character etc)

可以用四个或者五个参数，如果一个物体有能够设置的色调（如slash，character）的话就改成这个指定的色调

Example
```
/styleEntity ${1:defined entity name 定义的名字} ${2:red (0-1.0) 红} ${3:green (0-1.0) 绿} ${4:blue (0-1.0) 蓝}

/styleEntity ${1:defined entity name 定义的名字} ${2:red (0-1.0) 红} ${3:green (0-1.0) 绿} ${4:blue (0-1.0) 蓝} ${5:alpha (0-1.0) 透明度}
```
### /time
Sets a time in hours (this will adjust the lighting on entities), for example, 13.5 means 1:30 pm

设置游戏内光影的时间，（光影的改变只对物体生效）

Example
```
/time ${1:time 时间}
```

---------------------------------------------------------------------------------

## Examples

For example, if we want a game such that the protagonist keeps key memory after loading from save

例：如果主人公回档后会保留关键记忆

In this case, the protagonist was killed but doesn't know what did it in the first time, but just before dying, saw and recognized the killer's clothing.

比如主角最开始不知道是什么杀了他，但是死前认出了凶手的衣服

```
/define characterName ???
/setSpeakerName me
/if global::loop == 1
    /define characterName CharacterA?{TRANSLATE}角色A？
/if global::loop == 2
    /define characterName Psychopath{TRANSLATE}疯子
/if global::loop != 0
    Wait, what happened? did I just... what???{TRANSLATE}等等，刚才发生什么事了，我不是已经，啥？？？
    okay, calm down, calm down{TRANSLATE}冷静，冷静
    It was CharacterA, he took out a knife and stabbed me from the back{TRANSLATE}
    {TRANSLATE}是角色A，他刚刚从我的身后捅了我一刀
    /beginChoices 2
        /choice "Stay as if I knew nothing{TRANSLATE}以不变应万变"
            I decided to stay, I must been day dreaming{TRANSLATE}我一定是在做白日梦
            He's one of my best friend, it can't be true{TRANSLATE}他是我最好的朋友之一，怎么可能
        /choice Run{TRANSLATE}逃跑
            He's way too strong{TRANSLATE}他比我强壮太多了
            I should run before he tries anything{TRANSLATE}最好还是逃跑
    /endChoices

/setSpeakerName me
Its summer, I am so bored and have nothign to do{TRANSLATE}夏天真的无聊，无所事事
So I decided to invite some of my friends to a movie{TRANSLATE}所以我决定邀请一些朋友看电影
As I was standing here waiting, I seem to hear something{TRANSLATE}可是就在我站在这里等的时候
/setSpeakerName var::characterName
Nice and slow{TRANSLATE}慢慢的，慢慢的
/setSpeakerName me
what!??{TRANSLATE}什么！？？
Its you!? but{TRANSLATE}你是！？
Why did you……{TRANSLATE}你，为什……

% this is a comment
% character die effects here

/showUI false
/hold 1
/shake camera
/sound sound/falledOnGround
/hold 2
/showUI true

/set loop 1

/skipToDummy endOfGame

/setSpeakerName var::characterName
Nice and... wait up! where are you going{TRANSLATE}慢慢的……你别跑啊！
/setSpeakerName me
I ran and called the police{TRANSLATE}我赶忙逃跑并报了警
Turns out there's some crazy guy stabbing random people on the street{TRANSLATE}原来是有个疯子在街上见人就砍
He happens to wear the same hoodie as my friend{TRANSLATE}他刚好穿了和我朋友同款的衣服
My friend was stopped by the police because of the hoodie, that's why he kept me waiting{TRANSLATE}原来是因为撞衫了他被警察抓着盘问了，所以才让我等了呢么久
/set loop 2

/dummy endOfGame
The End{TRANSLATE}游戏结束

```

This will run as something like this (first game)

> me: Its summer, I am so bored and have nothign to do

> me: So I decided to invite some of my friends to a movie

> me: As I was standing here waiting, I seem to hear something

> ???: Nice and slow

> what!??

> Its you!? but

> Why did you……

> (Die effect)

> The End

In this run, the player don't know what stabbed him at all, so the name is now ???
这个存档中因为玩家不知道是谁刺了自己，所以名字是???

(second game)

> Wait, what happened? did I just... what???

> okay, calm down, calm down

> It was CharacterA, he took out a knife and stabbed me from the back

> Choice 1: Stay as if I knew nothing; Choice 2: Run;

> (Choice 2 selected)

> He's way too strong

> I should run before he tries anything

> CharacterA?: Nice and... wait up! where are you going

> me: So I ran and called the police

> me: Turns out there's some crazy guy stabbing random people on the street

> me: He happens to wear the same hoodie as my friend

> me: My friend was stopped by the police because of the hoodie, that's why he kept me waiting

Notice how in the second run the plot is different and the name of the character changed. In the third run, the name will be changed again now that the player knows that it is not his friend.

在这个轮回中，玩家会发现故事情节不太一样了，而且凶手名字变了，因为现在玩家认为凶手是自己的朋友，而到了第三个轮回，因为已经知道了凶手不是自己的朋友所以名字会再次发生改变。