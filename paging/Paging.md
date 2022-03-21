# Paging

## 定义数据源

### load

paging异步调用load函数

LoadParams对象保存两个信息：

- 要加载页面的键。第一次加载，LoadParams.key为null。

- 加载大小。请求加载内容的数量

### LoadResult

load方法的返回值。会有两种情况：

- LoadResult.Page 返回成功

  - 如果相应方向无数据，则给nextKey或prevKey设置为null
  - 例如：网络请求成功但是无数据，就可以给nextKey设置为null

- LoadResult.Error 发生错误

### getRefreshKey

刷新键用于对PagingSource.load的后续调用。

- 首次调用为初始加载，使用Pager提供的initialKey

- 每次Paging库要加载新数据来替代当前列表时，都会发生刷新

- 通常，后续刷新调用将需要重新开始加载以PagingState.anchorPosition（最近一次访问过的索引）为中心的数据

### 默认情况下，初始加载大小为3*页面大小。这样的Paging可确保首次加载列表时，用户可以看到足够的内容。如果用户滚动的范围没有超过已加载的内容，就不会触发过多的网络请求。计算PagingSource中的下一个键时，请务必牢记这一点

## 构建和配置PagingData

### 无论您使用哪种PagingData构建器，都必须传递以下参数

#### PagingCofig

该类用于设置关于如何从PagingSource加载内容的选项，例如提前多久加载、初始加载请求的大小，等等。

- 必须定义的唯一必需参数时**页面大小**，即应在每个页面中加载的项数。

- 默认情况下，Paging会将加载的所有页面保存在内存中。为确保系统在用户滚动时不会浪费内存，请在PagingConfig中设置maxSize参数。

- 默认情况下，如果Paging可以统计未加载项的数量以及enablePlaceholders配置标志为true，那么Paging将返回null作为未加载内容的占位符。如此，可以在适配器中显示占位符视图

#### 如何创建PagingSource的函数

### 注意

#### PagingConfig.pageSize应足够大，以满足多个不同大小的屏幕所要显示的内容。如果页面太小，由于页面内容未占据整个屏幕，列表可能会闪烁。页面越大，越有利于提高加载效率。但当列表更新时，可能会有延迟

#### 默认情况下，PagingConfig.maxSize是没有界限的，因此页面永远不会被丢弃，如果确定要丢弃页面，请确保将maxSize保持在一个足够大的数字，以免用户改变滚动方向时产生过多的网络请求，最小值为pageSize + prefetchDistance * 2

## 在viewModel中请求并缓存PagingData

### 使用```Flow<PagingData<Repo>>```可以帮助确保：每当有新的搜索结果与当前查询一致时，仅会返回当前的Flow，只有在新的查询和当前查询不同时，才需要调用拉取数据的方法

### ```Flow<PagingData>```有一个很方便的cachedIn方法，可以在CoroutineScope中缓存```Flow<PagingData>```的数据

- 如果想在Flow上进行一些操作，请务必在执行这些操作后调用cachedIn，从而确保无需再次出发这些操作

## 将适配器与PagingData配合使用

使用Paging库自带的PagingDataAdapter类

## 触发网络更新

### 协程相关

- 如要能使用```Flow<PagingData>```，需要启动一个新协程，当重新创建UI时，lifecycleScope负责取消请求。

- 如果希望确保用户搜索一个新的查询结果时，自动取消上一查询，就需要在repository中保留对Job的使用，每当搜索新的查询时，旧的job将被取消

## 在页脚中显示加载状态——暂未涉及

### 构筑的页眉/页脚应遵循如下原则：将相应列表附加在实际显示的列表项开头/结尾。页眉/页脚仅包含一个元素的列表，该元素根据PagingLoadState，显示进度条或带有重试按钮的错误信息

## 在activity中显示加载状态——暂未涉及

## 添加列表分隔符——暂未涉及