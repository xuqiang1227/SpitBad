大概就是，最外层设置一个relative的position，不设置高度，但是设置一个padding-bottom是一个 百分比，可以撑出一个高度(比例可以根据16:9或者4:3自己进行调整)
内层的iframe的width设置100%，高度也100%，然后position absolute 定位到那里就可以。

[Fluid Width Video](https://css-tricks.com/NetMag/FluidWidthVideo/Article-FluidWidthVideo.php)

[Creating Intrinsic Ratios for Video](http://alistapart.com/article/creating-intrinsic-ratios-for-video)
