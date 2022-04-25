# Bitmap和Drawable

## Bitmap转Drawable

### 使用KTX中的```bitmap.toDrawable()```

### 使用```BitmapDrawable(resources, this)```

## Drawable转Bitmap

### 使用KTX中的```drawable.toBitmap()```

## 什么是Bitmap？

> Bitmap就是位图信息的存储

## 什么是Drawable?

> 存储绘制规则的一个容器

- 正是因为它只是一个绘制规则，所以他的边界是需要绘制前手动赋值的```drawable.setBounds```

## 如何实现了互转？

### bitmap转Drawable

- 首先，这个过程实际上是创建了一个BitmapDrawable对象

- 这个BitmapDrawable对象的draw方法中，对这个bitmap进行了绘制（使用draw方法）

### drawable转bitmap

- 首先，如果这个Drawable本身就是一个BitmapDrawable，那么它当中一定存储了一份bitmap，我们只要取出即可

- 如果他不是，那我们可以将它绘制到canvas上，然后返回对应的bitmap

- 使用```drawable.draw(Canvas(bitmap))```就可以将drawable绘制到bitmap上

### 严格而言这就不是互转，而是使用一个对象创建出了另一个对象

## 自定义Drawable

### setAlpha

由于我们想要绘制，就肯定会持有一个paint画笔，所以我们的setAlpha就是在对paint操作，即调用paint的setAlpha

### setColorFilter

和setAlpha类似

### getOpacity(课外学习)

### onDraw

使用canvas进行绘制

## 自定义Drawable有什么用

方便复用