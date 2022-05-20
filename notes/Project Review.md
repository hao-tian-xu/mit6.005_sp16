# <u>Project. ABC Player</u>

### **Make the whole program work correctly first.**

- Skip the **advanced features** for now. 
- Skip **performance optimization**. 
- Skip **corner cases**. 
- 不要追求一致性，先完成基本功能
- 过程中多做Test，Experiment，不要等到写了大段的语句再做
- **做任何修改之前先确定不修改是否影响程序功能**
- Keep **a to-do list of what you have to revisit**.

#### **Iterate**

- make it work better. 
- Reimplement, 
- optimize, 
- redesign if necessary.



## Test

When I have private methods in a class that are sufficiently complicated that I feel the need to test the private methods directly, that is a code smell: my class is too complicated.



## *Reflection*

### [Team Contract](http://web.mit.edu/6.005/www/sp16/projects/abcplayer/team-contract/)

这是一个合作的项目，虽然我是独自完成的，但过程中也能感觉到一些团队合作的点，一个contract显然是必要的，但是在开始之前能够切实分析各部分的工作量和spec也是一件不容易的事

一个Contract在这里包含目标，会议和交流的准则，工作的准则（包含工作时长，工作分配，如何记录，特殊情况的处理等等，尽可能详细），决策机制等

在Contract上花时间显然是有价值的，毕竟往后的大量时间和工作都要在这个contract的约束下进行

### [Design Freedom](http://web.mit.edu/6.005/www/sp16/projects/abcplayer/#project_abc_music_player)

- Do we need to check the input for errors? What should we do if there are errors?
- Can tuplets or chords contain notes of different lengths?
- Should we handle nested repeats?
- What if two voices contain a different number of total bars?

…and there are many, many more. ***And there is unlikely to be a single hard-and-fast answer.***

### [Reflection](http://web.mit.edu/6.005/www/sp16/projects/abcplayer/reflection/) on myself

使用了七天的时间8.18-8.24，平均每天6个小时，确实是一个长期的工作。最后的成果来说还是不错的，应该能拿到A～B这样。

所有的specification，test，和implementation都是独自完成的。

- test针对AbcParser进行了完整的单元化，但没有对输入输出做完整的partition，这个test中对Music和MusicPiece都做了完整的调用，因此没有再针对Music和MusicPiece写test。
- specification做到了内容的完整，但***没有做到输入输出的各种情况的分析***

#### **未来项目中想要坚持，开始，或停止的习惯**

- 坚持或开始
	- 开始：事先对整个项目进行完整详尽的分析，完全理解了项目的结构并设计出可行的程序结构后，再进行代码实现
		- 这需要对软件工程本身有较高的熟悉程度
		- 也需要对乐谱有比较全面的了解
		- 对于一个7天的项目，抽出完整的1-2天来分析和设计结构是合理的
	- 坚持：首先让你在工作的部分正确运行，其次才是完善
		- 高级特性、性能优化、边角案例、功能实现的一致性等等都暂时放置一边
			- 将所有这些问题存放在一个to-do list里
		- 乃至整体的结构，例如单个还是多个class，都可以后续再refactor
- 停止
	- 停止：做完成功能方面没有必要的修改，还会出现很多反复
	- 停止：写一个过于复杂的class，包含大量应该是private的method，但是犹豫test的需要，不得不调整为no modifier（package-private）
		- 一个复杂的单一功能的module/package，是否应该写为多个class，例如AbcParser，把输入的乐谱转化为Music DT，中间基本都是针对ParseTree的操作，分开成多个class如果只是为了方便测试，那么package-private同样可以测试
		- 分解为小class应该更有利于团队合作和contract的形成
			- 一个单独的class，则意味着private的方法不需要spec，在复杂的项目中使得测试和debug变得十分困难
- 犹豫
	- 上来就完成完整的spec和white-box test
		- spec的关键作用还是将具体实现和contract分离，而黑盒测试则是保证这个contract的方法
		- 你也发现在后期进行implementation的调整时，test是保证contract的关键，同样的，进行spec调整后，test也要相应的调整

### Re-design the Project

- 乐谱结构
	- header
	- body
		- 声部：一个或多个声部同时演奏
		- 段落：一个或多个，受反复记号的影响
		- 小节：受变音记号的影响
		- 音符

- 程序结构
	- Grammar: for parsing abc files
		- music由header和body组成
		- body分成一个或多个声部（通过header分析声部数量和名称）
		- 声部为一个或多个段落
		- 三类段落：普通，普通重复，不同结尾重复（均由小节和不同的小节线构成）
		- 小节为一个或多个音符元素
		- 四类音符元素：单音符，休止符，和弦，连音（均由音符构成）
		- 音符的格式（变音记号？ 基础音符 八度？）
	- Immutable ADT (an AST): for music
		- Music：一个或多个声部 + 音乐信息
		- Music Piece（AST）：一个声部
			- Concat（包含Tuplet）
			- Chord：开始时间相同的多个Note
			- Note
			- Rest
	- Module: to convert parse trees into ASTs
		- music分为header和body
			- header信息分析后储存
			- body分析为多个声部
				- 声部分析为多个段落Concat
					- 处理段落的重复
					- 处理后的段落分析为多个小节Concat
						- 处理小节的变音记号
						- 处理后的小节分析为多个音符元素Concat
							- 处理音符元素
								- Tuplet：调整每个元素的时长后Concat
		- class
			- AbcParser
			- Header
			- Voice
			- Bar
			- Number
	- Module: sends music to the MIDI player
		- 用同样的开始时间，添加Music中的每一个声部（MusicPiece）到Sequence Player里
			- MusicPiece实现一个递归的添加音符到Sequence Player里的方法













































