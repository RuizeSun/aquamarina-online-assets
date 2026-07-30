# Aquamarina Online Assets

英语学习在线静态资源库，为 [Aquamarina](https://aquamarina.78go.work) 项目提供句型集（Corpus）和词书（Wordbook）数据。

部署于 `https://assets.aquamarina.78go.work/`，所有 JSON 文件均可通过 `fetch()` 直接获取。

---

## 目录结构

```
/
├── index.json                     # 根入口
├── corpus/                        # 句型集（句子语料库）
│   ├── index.json                 # 句型集分类索引
│   └── dailyconversation/         # 示例分类：日常对话
│       ├── index.json             # 该分类下的语料条目索引
│       └── basic-greeting.json    # 具体语料数据
├── wordbook/                      # 词书（单词/词组列表）
│   ├── index.json                 # 词书分类索引
│   └── textbook/                  # 示例分类：教材词汇
│       ├── index.json             # 该分类下的词书条目索引
│       └── newpep-9vol1.json      # 具体词书数据
└── README.md
```

---

## 根入口：`/index.json`

仓库的总入口文件，描述了版本号及两个数据模块的路径。

```json
{
	"version": 1,
	"text": "Online Assets for Aquamarina.",
	"corpus": "/corpus/index.json",
	"wordbook": "/wordbook/index.json"
}
```

| 字段       | 类型     | 说明           |
| ---------- | -------- | -------------- |
| `version`  | `number` | 数据版本号     |
| `text`     | `string` | 项目描述       |
| `corpus`   | `string` | 句型集索引路径 |
| `wordbook` | `string` | 词书索引路径   |

---

## 句型集（Corpus）数据结构

句型集共分三级索引：

### 第一级：分类索引 `/corpus/index.json`

按类别组织，每个类别包含双语标题和指向该类别子索引的路径。

```json
{
	"categlories": [
		{
			"title": {
				"zh-CN": "日常对话",
				"en-US": "Daily Conversation"
			},
			"index": "/corpus/dailyconversation/index.json"
		}
	]
}
```

| 字段                  | 类型     | 说明                                         |
| --------------------- | -------- | -------------------------------------------- |
| `categlories`         | `array`  | 分类列表                                     |
| `categlories[].title` | `object` | 双语标题，`zh-CN` 为中文名，`en-US` 为英文名 |
| `categlories[].index` | `string` | 该分类子索引的路径（相对于根目录）           |

### 第二级：分类下的条目索引 `/corpus/{category}/index.json`

每个分类下的语料文件列表，每个条目包含标题、作者和文件名。

```json
[
	{
		"title": {
			"zh-CN": "基本问候",
			"en-US": "Basic Greeting"
		},
		"author": {
			"en-US": "Ruize Sun"
		},
		"file": "basic-greeting.json"
	}
]
```

| 字段     | 类型     | 说明                                                   |
| -------- | -------- | ------------------------------------------------------ |
| `title`  | `object` | 该语料集的双语标题                                     |
| `author` | `object` | 作者信息，目前使用 `en-US` 字段                        |
| `file`   | `string` | 语料数据文件名，完整路径为 `/corpus/{category}/{file}` |

### 第三级：具体语料数据 `/corpus/{category}/{file}`

JSON 数组，每个元素包含英文句子、中文翻译和干扰词列表。

```json
[
	{
		"en": "Hello, how are you?",
		"zh": "你好，你好吗？",
		"dw": ["good", "bad", "sad", "tired", "happy"]
	},
	{
		"en": "Good morning, have a nice day!",
		"zh": "早上好，祝你有美好的一天！",
		"dw": ["night", "cold", "hot", "rainy", "windy"]
	}
]
```

| 字段 | 类型       | 说明                                               |
| ---- | ---------- | -------------------------------------------------- |
| `en` | `string`   | 英文句子                                           |
| `zh` | `string`   | 中文翻译                                           |
| `dw` | `string[]` | 干扰词列表（Distract Words），可用于生成选择题选项 |

---

## 词书（Wordbook）数据结构

词书也采用三级索引结构：

### 第一级：分类索引 `/wordbook/index.json`

```json
{
	"categlories": [
		{
			"title": {
				"zh-CN": "教材词汇",
				"en-US": "Textbook Vocabulary"
			},
			"index": "/wordbook/textbook/index.json"
		}
	]
}
```

结构同 Corpus 第一级。

| 字段                  | 类型     | 说明             |
| --------------------- | -------- | ---------------- |
| `categlories`         | `array`  | 分类列表         |
| `categlories[].title` | `object` | 双语标题         |
| `categlories[].index` | `string` | 该分类子索引路径 |

### 第二级：分类下的词书索引 `/wordbook/{category}/index.json`

```json
[
	{
		"title": {
			"zh-CN": "新人教版九年级上册英语",
			"en-US": "PEP Grade 9 Volume 1 (2026)"
		},
		"author": {
			"en-US": "Ruize Sun"
		},
		"file": "newpep-9vol1.json"
	}
]
```

| 字段     | 类型     | 说明                                                     |
| -------- | -------- | -------------------------------------------------------- |
| `title`  | `object` | 词书的双语标题                                           |
| `author` | `object` | 作者信息                                                 |
| `file`   | `string` | 词书数据文件名，完整路径为 `/wordbook/{category}/{file}` |

### 第三级：具体词书数据 `/wordbook/{category}/{file}`

一个纯字符串数组，每个元素为一个单词或短语。

```json
["bring about", "rough", "be covered with", "sandstorm", "farmland", "shortage"]
```

> **注意**：数组中包含带空格的词组（如 `"bring about"`、`"be covered with"`），搜索时需以**子串匹配**方式处理，而非按空格分词匹配。

---

## 数据请求方式

所有资源部署在 `https://assets.aquamarina.78go.work/`，直接通过 URL 路径使用 `fetch` 请求即可。

```javascript
// 获取根入口
const root = await fetch("https://assets.aquamarina.78go.work/index.json").then((r) => r.json());

// 获取 Corpus 分类索引
const corpusIndex = await fetch("https://assets.aquamarina.78go.work/corpus/index.json").then((r) => r.json());

// 获取 Corpus 子分类下的条目索引
const dailyConvIndex = await fetch("https://assets.aquamarina.78go.work/corpus/dailyconversation/index.json").then((r) => r.json());

// 获取具体语料数据
const basicGreeting = await fetch("https://assets.aquamarina.78go.work/corpus/dailyconversation/basic-greeting.json").then((r) => r.json());

// 获取词书数据
const wordbookData = await fetch("https://assets.aquamarina.78go.work/wordbook/textbook/newpep-9vol1.json").then((r) => r.json());
```

所有 JSON 文件都是静态资源，支持 CDN 缓存，适合高频读取。

---

## 搜索实现

搜索的核心思路：从索引出发，遍历所有词书/句型集，逐层递归加载数据，对每条内容做子串匹配。

### 搜索词书

在词书中搜索时，遍历所有词书分类 → 遍历分类下所有词书文件 → 加载 JSON 数组 → 对每个单词/短语做子串匹配。

```javascript
async function searchWordbook(keyword) {
	const results = [];

	// 1. 获取词书分类索引
	const wbRoot = await fetch("https://assets.aquamarina.78go.work/wordbook/index.json").then((r) => r.json());

	for (const category of wbRoot.categlories) {
		// 2. 获取该分类下的词书条目列表
		const catIndex = await fetch(`https://assets.aquamarina.78go.work${category.index}`).then((r) => r.json());

		for (const entry of catIndex) {
			// 3. 加载具体词书数据
			const dir = category.index.replace("/index.json", "");
			const wordList = await fetch(`https://assets.aquamarina.78go.work${dir}/${entry.file}`).then((r) => r.json());

			// 4. 遍历每个单词做子串匹配（忽略大小写）
			const lowerKeyword = keyword.toLowerCase();
			for (const word of wordList) {
				if (word.toLowerCase().includes(lowerKeyword)) {
					results.push({
						word,
						source: entry.title,
						category: category.title,
					});
				}
			}
		}
	}

	return results;
}
```

**匹配规则**：

- 不区分大小写（`toLowerCase()`）
- 子串匹配（`includes()`），因此搜索 `"bring"` 可以匹配到 `"bring about"`

### 搜索句型集（Corpus）

在句型集中搜索时，遍历所有分类 → 遍历分类下所有语料文件 → 加载 JSON 数组 → 对每条语料的 `en` 和 `zh` 字段做子串匹配。

```javascript
async function searchCorpus(keyword) {
	const results = [];

	// 1. 获取句型集分类索引
	const corpusRoot = await fetch("https://assets.aquamarina.78go.work/corpus/index.json").then((r) => r.json());

	for (const category of corpusRoot.categlories) {
		// 2. 获取该分类下的语料条目列表
		const catIndex = await fetch(`https://assets.aquamarina.78go.work${category.index}`).then((r) => r.json());

		for (const entry of catIndex) {
			// 3. 加载具体语料数据
			const dir = category.index.replace("/index.json", "");
			const sentences = await fetch(`https://assets.aquamarina.78go.work${dir}/${entry.file}`).then((r) => r.json());

			// 4. 遍历每条语料，在 en 和 zh 中做子串匹配
			const lowerKeyword = keyword.toLowerCase();
			for (const item of sentences) {
				if (
					item.en.toLowerCase().includes(lowerKeyword) ||
					item.zh.includes(keyword) // 中文不需要 toLowerCase
				) {
					results.push({
						en: item.en,
						zh: item.zh,
						dw: item.dw,
						source: entry.title,
						category: category.title,
					});
				}
			}
		}
	}

	return results;
}
```

**匹配规则**：

- 英文部分：不区分大小写的子串匹配
- 中文部分：直接子串匹配（中文没有大小写问题）

### 统一搜索入口

如果需要同时搜索词书和句型集，可以合并结果：

```javascript
async function searchAll(keyword) {
	const [wordbookResults, corpusResults] = await Promise.all([searchWordbook(keyword), searchCorpus(keyword)]);

	return {
		keyword,
		wordbook: wordbookResults,
		corpus: corpusResults,
	};
}
```

### 搜索优化建议

- **缓存索引**：一级和二级索引数据量很小，可在应用启动时一次性加载并缓存
- **按需加载三级数据**：如果词书/句型集数量较多，可在首次搜索时才动态加载具体文件
- **请求合并**：对于需要频繁搜索的场景，可考虑将所有的词书/句型集数据在应用初始化时预加载到本地缓存

---

## 扩展指南

### 添加新的句型集

1. 在 `corpus/` 下新建分类目录，例如 `corpus/business/`
2. 在该目录下创建 `index.json`，编写条目列表
3. 创建实际的语料数据 JSON 文件，例如 `corpus/business/meetings.json`
4. 在 `corpus/index.json` 的 `categlories` 中添加新分类的引用

### 添加新的词书

1. 在 `wordbook/` 下新建分类目录，例如 `wordbook/exam/`
2. 在该目录下创建 `index.json`，编写条目列表
3. 创建实际的词书数据 JSON 文件，例如 `wordbook/exam/cet4.json`
4. 在 `wordbook/index.json` 的 `categlories` 中添加新分类的引用

---

## License

MIT
