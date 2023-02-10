![1.gif](https://github.com/CZXBigBrother/flutter_lucky_wheel/blob/master/1.gif)
功能实现起来其实非常简单,将下面三张图依次叠加起来,然后让指针转动或者奖励转动就可以了
![image.png](https://github.com/CZXBigBrother/flutter_lucky_wheel/blob/master/3.png)
先上代码
```
  @override
  void initState() {
    super.initState();
    animationController = AnimationController(
      vsync: this,
      duration: const Duration(milliseconds: 2000),
    );
    animationController.addListener(() {
      if (animationController.status == AnimationStatus.completed) {
        isRunning = false;
        //结束了
      } else if (animationController.status == AnimationStatus.forward) {
        // print('forward');
      } else if (animationController.status == AnimationStatus.reverse) {
        // print('reverse');
      }
    });
    // 创建一个从0到360弧度的补间动画 v * 2 * π
  }

  void startAnimation(int index) {
    _animation = Tween<double>(begin: 0, end: 2 + 0.125 * index)
        .animate(CurvedAnimation(
      parent: animationController,
      curve: const Interval(
        0,
        1.0,
        curve: Curves.ease,
      ),
    ));
    setState(() {});
    animationController.reset();
    animationController.forward();
  }
```
我们提供的素材是有8个奖品.所以我们最终的落点是 ```0.125 * index```,但是我们之前要想多转两圈再停止所以前面先加```2```,可以根据设计给的图片改变其实旋转的圈数和最终落点的角度,修改角度的正负号就可以实现向左转动还是向右转动
```
Widget wheelView() {
    return SizedBox(
      width: 300,
      height: 300,
      child: Stack(
        children: [
          Align(
            alignment: Alignment.center,
            child: Image.asset(
              'assets/lucky_wheel/bg_zhuanpan.png',
              width: 300,
            ),
          ),
          Align(
            alignment: Alignment.center,
            child: RotationTransition(
                turns: _animation,
                child: Image.asset(
                  'assets/lucky_wheel/bg_superjiangpin.png',
                  width: 290,
                )),
          ),
          Align(
            alignment: Alignment.center,
            child: GestureDetector(
              onTap: buttonOnClickStartRun,
              child: Image.asset(
                'assets/lucky_wheel/ic_pointer.png',
                width: 150,
              ),
            ),
          ),
        ],
      ),
    );
  }
```
最后是控件本身非常简单,如果想转盘转,就把bg_superjiangpin设置动画,如果想指针转就用ic_pointer设置动画


