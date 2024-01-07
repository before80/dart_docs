+++
title = "dart:html"
date = 2024-01-05T20:29:36+08:00
weight = 60
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://dart.dev/libraries/dart-html](https://dart.dev/libraries/dart-html)

## dart:html

Use the [dart:html](https://api.dart.dev/stable/dart-html/dart-html-library.html) library to program the browser, manipulate objects and elements in the DOM, and access HTML5 APIs. DOM stands for *Document Object Model*, which describes the hierarchy of an HTML page.

​	使用 dart:html 库对浏览器进行编程，操作 DOM 中的对象和元素，并访问 HTML5 API。DOM 代表文档对象模型，它描述了 HTML 页面的层次结构。

Other common uses of dart:html are manipulating styles (*CSS*), getting data using HTTP requests, and exchanging data using [WebSockets](https://dart.dev/libraries/dart-html#sending-and-receiving-real-time-data-with-websockets). HTML5 (and dart:html) has many additional APIs that this section doesn’t cover. Only web apps can use dart:html, not command-line apps.

​	dart:html 的其他常见用途包括操作样式 (CSS)、使用 HTTP 请求获取数据以及使用 WebSocket 交换数据。HTML5（和 dart:html）还有许多其他 API，本节未予介绍。只有网络应用才能使用 dart:html，命令行应用不能使用。

*info* **Note:** For larger applications or if you already have a Flutter application, consider using [Flutter for web.](https://flutter.dev/web)

​	注意：对于较大的应用程序或如果您已有 Flutter 应用程序，请考虑使用 Flutter 进行网络开发。

To use the HTML library in your web app, import dart:html:

​	要在您的网络应用中使用 HTML 库，请导入 dart:html：

```dart
import 'dart:html';
```

### 操作 DOM - Manipulating the DOM 

To use the DOM, you need to know about *windows*, *documents*, *elements*, and *nodes*.

​	要使用 DOM，您需要了解窗口、文档、元素和节点。

A [Window](https://api.dart.dev/stable/dart-html/Window-class.html) object represents the actual window of the web browser. Each Window has a Document object, which points to the document that’s currently loaded. The Window object also has accessors to various APIs such as IndexedDB (for storing data), requestAnimationFrame (for animations), and more. In tabbed browsers, each tab has its own Window object.

​	Window 对象表示网络浏览器的实际窗口。每个 Window 都具有一个 Document 对象，该对象指向当前加载的文档。Window 对象还可以访问各种 API，例如 IndexedDB（用于存储数据）、requestAnimationFrame（用于动画）等。在标签式浏览器中，每个标签都有自己的 Window 对象。

With the [Document](https://api.dart.dev/stable/dart-html/Document-class.html) object, you can create and manipulate [Element](https://api.dart.dev/stable/dart-html/Element-class.html) objects within the document. Note that the document itself is an element and can be manipulated.

​	使用 Document 对象，您可以在文档中创建和操作 Element 对象。请注意，文档本身是一个元素，可以进行操作。

The DOM models a tree of [Nodes.](https://api.dart.dev/stable/dart-html/Node-class.html) These nodes are often elements, but they can also be attributes, text, comments, and other DOM types. Except for the root node, which has no parent, each node in the DOM has one parent and might have many children.

​	DOM 对节点树进行建模。这些节点通常是元素，但它们也可以是属性、文本、注释和其他 DOM 类型。除了没有父级的根节点外，DOM 中的每个节点都有一个父级，并且可能有多个子级。

#### 查找元素 Finding elements 

To manipulate an element, you first need an object that represents it. You can get this object using a query.

​	要操作元素，您首先需要一个表示它的对象。您可以使用查询获取此对象。

Find one or more elements using the top-level functions `querySelector()` and `querySelectorAll()`. You can query by ID, class, tag, name, or any combination of these. The [CSS Selector Specification guide](https://www.w3.org/TR/css3-selectors/) defines the formats of the selectors such as using a # prefix to specify IDs and a period (.) for classes.

​	使用顶级函数 `querySelector()` 和 `querySelectorAll()` 查找一个或多个元素。您可以按 ID、类、标签、名称或这些的任意组合进行查询。CSS 选择器规范指南定义了选择器的格式，例如使用 # 前缀指定 ID，使用句点 (.) 指定类。

The `querySelector()` function returns the first element that matches the selector, while `querySelectorAll()`returns a collection of elements that match the selector.

​	函数 `querySelector()` 返回与选择器匹配的第一个元素，而 `querySelectorAll()` 返回与选择器匹配的元素集合。

```dart
// Find an element by id (an-id).
Element idElement = querySelector('#an-id')!;

// Find an element by class (a-class).
Element classElement = querySelector('.a-class')!;

// Find all elements by tag (<div>).
List<Element> divElements = querySelectorAll('div');

// Find all text inputs.
List<Element> textInputElements = querySelectorAll(
  'input[type="text"]',
);

// Find all elements with the CSS class 'class'
// inside of a <p> that is inside an element with
// the ID 'id'.
List<Element> specialParagraphElements = querySelectorAll('#id p.class');
```

#### 操作元素 Manipulating elements 

You can use properties to change the state of an element. Node and its subtype Element define the properties that all elements have. For example, all elements have `classes`, `hidden`, `id`, `style`, and `title` properties that you can use to set state. Subclasses of Element define additional properties, such as the `href` property of [AnchorElement.](https://api.dart.dev/stable/dart-html/AnchorElement-class.html)

​	您可以使用属性来更改元素的状态。Node 及其子类型 Element 定义了所有元素具有的属性。例如，所有元素都具有 `classes` 、 `hidden` 、 `id` 、 `style` 和 `title` 属性，您可以使用这些属性来设置状态。Element 的子类定义了其他属性，例如 AnchorElement 的 `href` 属性。

Consider this example of specifying an anchor element in HTML:

​	考虑在 HTML 中指定锚元素的此示例：

```html
<a id="example" href="/another/example">link text</a>
```

This `<a>` tag specifies an element with an `href` attribute and a text node (accessible via a `text` property) that contains the string “link text”. To change the URL that the link goes to, you can use AnchorElement’s `href` property:

​	此 `<a>` 标签指定了一个具有 `href` 属性和文本节点（可通过 `text` 属性访问）的元素，该文本节点包含字符串“链接文本”。要更改链接指向的 URL，可以使用 AnchorElement 的 `href` 属性：

```dart
var anchor = querySelector('#example') as AnchorElement;
anchor.href = 'https://dart.dev';
```

Often you need to set properties on multiple elements. For example, the following code sets the `hidden` property of all elements that have a class of “mac”, “win”, or “linux”. Setting the `hidden` property to true has the same effect as adding `display: none` to the CSS.

​	通常，您需要在多个元素上设置属性。例如，以下代码设置了所有具有“mac”、“win”或“linux”类的元素的 `hidden` 属性。将 `hidden` 属性设置为 true 与将 `display: none` 添加到 CSS 的效果相同。

```html
<!-- In HTML: -->
<p>
  <span class="linux">Words for Linux</span>
  <span class="macos">Words for Mac</span>
  <span class="windows">Words for Windows</span>
</p>
// In Dart:
const osList = ['macos', 'windows', 'linux'];
final userOs = determineUserOs();

// For each possible OS...
for (final os in osList) {
  // Matches user OS?
  bool shouldShow = (os == userOs);

  // Find all elements with class=os. For example, if
  // os == 'windows', call querySelectorAll('.windows')
  // to find all elements with the class "windows".
  // Note that '.$os' uses string interpolation.
  for (final elem in querySelectorAll('.$os')) {
    elem.hidden = !shouldShow; // Show or hide.
  }
}
```

When the right property isn’t available or convenient, you can use Element’s `attributes` property. This property is a `Map<String, String>`, where the keys are attribute names. For a list of attribute names and their meanings, see the [MDN Attributes page.](https://developer.mozilla.org/docs/Web/HTML/Attributes) Here’s an example of setting an attribute’s value:

​	当正确的属性不可用或不方便时，您可以使用 Element 的 `attributes` 属性。此属性是一个 `Map<String, String>` ，其中键是属性名称。有关属性名称及其含义的列表，请参阅 MDN 属性页面。以下是如何设置属性值的示例：

```dart
elem.attributes['someAttribute'] = 'someValue';
```

#### 创建元素 Creating elements 

You can add to existing HTML pages by creating new elements and attaching them to the DOM. Here’s an example of creating a paragraph (`<p>`) element:

​	您可以通过创建新元素并将其附加到 DOM 来添加到现有 HTML 页面。以下是如何创建一个段落 (`<p>`) 元素的示例：

```dart
var elem = ParagraphElement();
elem.text = 'Creating is easy!';
```

You can also create an element by parsing HTML text. Any child elements are also parsed and created.

​	您还可以通过解析 HTML 文本来创建元素。任何子元素也会被解析并创建。

```dart
var elem2 = Element.html(
  '<p>Creating <em>is</em> easy!</p>',
);
```

Note that `elem2` is a `ParagraphElement` in the preceding example.

​	请注意，在前面的示例中， `elem2` 是一个 `ParagraphElement` 。

Attach the newly created element to the document by assigning a parent to the element. You can add an element to any existing element’s children. In the following example, `body` is an element, and its child elements are accessible (as a `List<Element>`) from the `children` property.

​	通过为元素分配父元素来将新创建的元素附加到文档。您可以将元素添加到任何现有元素的子元素。在以下示例中， `body` 是一个元素，并且可以从 `children` 属性访问其子元素（作为 `List<Element>` ）。

```dart
document.body!.children.add(elem2);
```

#### 添加、替换和删除节点 Adding, replacing, and removing nodes 

Recall that elements are just a kind of node. You can find all the children of a node using the `nodes` property of Node, which returns a `List<Node>` (as opposed to `children`, which omits non-Element nodes). Once you have this list, you can use the usual List methods and operators to manipulate the children of the node.

​	回想一下，元素只是节点的一种。您可以使用 Node 的 `nodes` 属性查找节点的所有子节点，该属性返回一个 `List<Node>` （与 `children` 相反，后者会省略非元素节点）。获得此列表后，您可以使用常规的 List 方法和运算符来操作节点的子节点。

To add a node as the last child of its parent, use the List `add()` method:

​	要将节点添加为其父节点的最后一个子节点，请使用 List `add()` 方法：

```dart
querySelector('#inputs')!.nodes.add(elem);
```

To replace a node, use the Node `replaceWith()` method:

​	要替换节点，请使用 Node `replaceWith()` 方法：

```dart
querySelector('#status')!.replaceWith(elem);
```

To remove a node, use the Node `remove()` method:

​	要移除节点，请使用 Node `remove()` 方法：

```dart
// Find a node by ID, and remove it from the DOM if it is found.
querySelector('#expendable')?.remove();
```

#### 操作 CSS 样式 Manipulating CSS styles 

CSS, or *cascading style sheets*, defines the presentation styles of DOM elements. You can change the appearance of an element by attaching ID and class attributes to it.

​	CSS 或层叠样式表定义了 DOM 元素的显示样式。您可以通过为元素附加 ID 和 class 属性来更改元素的外观。

Each element has a `classes` field, which is a list. Add and remove CSS classes simply by adding and removing strings from this collection. For example, the following sample adds the `warning` class to an element:

​	每个元素都有一个 `classes` 字段，它是一个列表。只需向此集合添加和移除字符串即可添加和移除 CSS 类。例如，以下示例将 `warning` 类添加到元素：

```dart
var elem = querySelector('#message')!;
elem.classes.add('warning');
```

It’s often very efficient to find an element by ID. You can dynamically set an element ID with the `id` property:

​	通常，按 ID 查找元素非常有效。您可以使用 `id` 属性动态设置元素 ID：

```dart
var message = DivElement();
message.id = 'message2';
message.text = 'Please subscribe to the Dart mailing list.';
```

You can reduce the redundant text in this example by using method cascades:

​	您可以使用方法级联来减少此示例中的冗余文本：

```dart
var message = DivElement()
  ..id = 'message2'
  ..text = 'Please subscribe to the Dart mailing list.';
```

While using IDs and classes to associate an element with a set of styles is best practice, sometimes you want to attach a specific style directly to the element:

​	虽然使用 ID 和类将元素与一组样式相关联是最佳做法，但有时您希望将特定样式直接附加到元素：

```dart
message.style
  ..fontWeight = 'bold'
  ..fontSize = '3em';
```

#### 处理事件 Handling events 

To respond to external events such as clicks, changes of focus, and selections, add an event listener. You can add an event listener to any element on the page. Event dispatch and propagation is a complicated subject; [research the details](https://www.w3.org/TR/DOM-Level-3-Events/#dom-event-architecture) if you’re new to web programming.

​	要响应外部事件（如点击、焦点变化和选择），请添加事件侦听器。您可以将事件侦听器添加到页面上的任何元素。事件分派和传播是一个复杂的问题；如果您是 Web 编程新手，请研究详细信息。

Add an event handler using `element.onEvent.listen(function)`, where `Event` is the event name and `function` is the event handler.

​	使用 `element.onEvent.listen(function)` 添加事件处理程序，其中 `Event` 是事件名称， `function` 是事件处理程序。

For example, here’s how you can handle clicks on a button:

​	例如，以下是如何处理按钮上的点击：

```dart
// Find a button by ID and add an event handler.
querySelector('#submitInfo')!.onClick.listen((e) {
  // When the button is clicked, it runs this code.
  submitData();
});
```

Events can propagate up and down through the DOM tree. To discover which element originally fired the event, use `e.target`:

​	事件可以在 DOM 树中向上和向下传播。要发现最初触发事件的元素，请使用 `e.target` ：

```dart
document.body!.onClick.listen((e) {
  final clickedElem = e.target;
  // ...
});
```

To see all the events for which you can register an event listener, look for “onEventType” properties in the API docs for [Element](https://api.dart.dev/stable/dart-html/Element-class.html) and its subclasses. Some common events include:

​	要查看可以为其注册事件侦听器的所有事件，请在 Element 及其子类的 API 文档中查找“onEventType”属性。一些常见事件包括：

- change
- blur
- keyDown
- keyUp
- mouseDown
- mouseUp

### 使用 HttpRequest 与 HTTP 资源进行交互 Using HTTP resources with HttpRequest 

You should avoid directly using `dart:html` to make HTTP requests. The [`HttpRequest`](https://api.dart.dev/stable/dart-html/HttpRequest-class.html) class in `dart:html` is platform-dependent and tied to a single implementation. Instead, use a higher-level library like [`package:http`](https://pub.dev/packages/http).

​	您应该避免直接使用 `dart:html` 来发出 HTTP 请求。 `HttpRequest` 类在 `dart:html` 中是与平台相关的，并且绑定到单个实现。相反，请使用更高级别的库，例如 `package:http` 。

The [Fetch data from the internet](https://dart.dev/tutorials/server/fetch-data) tutorial explains how to make HTTP requests using `package:http`.

​	从互联网获取数据教程解释了如何使用 `package:http` 发出 HTTP 请求。

### 使用 WebSocket 发送和接收实时数据 Sending and receiving real-time data with WebSockets 

A WebSocket allows your web app to exchange data with a server interactively—no polling necessary. A server creates the WebSocket and listens for requests on a URL that starts with **ws://**—for example, ws://127.0.0.1:1337/ws. The data transmitted over a WebSocket can be a string or a blob. Often, the data is a JSON-formatted string.

​	WebSocket 允许您的网络应用与服务器交互式地交换数据，无需轮询。服务器创建 WebSocket 并侦听以 ws:// 开头的 URL 上的请求，例如 ws://127.0.0.1:1337/ws。通过 WebSocket 传输的数据可以是字符串或 Blob。通常，数据是 JSON 格式的字符串。

To use a WebSocket in your web app, first create a [WebSocket](https://api.dart.dev/stable/dart-html/WebSocket-class.html) object, passing the WebSocket URL as an argument:

​	要在您的网络应用中使用 WebSocket，首先创建一个 WebSocket 对象，将 WebSocket URL 作为参数传递：

```dart
var ws = WebSocket('ws://echo.websocket.org');
```

#### 发送数据 Sending data 

To send string data on the WebSocket, use the `send()` method:

​	要在 WebSocket 上发送字符串数据，请使用 `send()` 方法：

```dart
ws.send('Hello from Dart!');
```

#### 接收数据 Receiving data 

To receive data on the WebSocket, register a listener for message events:

​	要在 WebSocket 上接收数据，请为消息事件注册一个侦听器：

```dart
ws.onMessage.listen((MessageEvent e) {
  print('Received message: ${e.data}');
});
```

The message event handler receives a [MessageEvent](https://api.dart.dev/stable/dart-html/MessageEvent-class.html) object. This object’s `data` field has the data from the server.

​	消息事件处理程序接收 MessageEvent 对象。此对象的 `data` 字段包含来自服务器的数据。

#### 处理 WebSocket 事件 Handling WebSocket events 

Your app can handle the following WebSocket events: open, close, error, and (as shown earlier) message. Here’s an example of a method that creates a WebSocket object and registers handlers for open, close, error, and message events:

​	您的应用可以处理以下 WebSocket 事件：open、close、error 以及（如前文所示）message。以下是一个创建 WebSocket 对象并为 open、close、error 和 message 事件注册处理程序的方法示例：

```dart
void initWebSocket([int retrySeconds = 1]) {
  var reconnectScheduled = false;

  print('Connecting to websocket');

  void scheduleReconnect() {
    if (!reconnectScheduled) {
      Timer(Duration(seconds: retrySeconds),
          () => initWebSocket(retrySeconds * 2));
    }
    reconnectScheduled = true;
  }

  ws.onOpen.listen((e) {
    print('Connected');
    ws.send('Hello from Dart!');
  });

  ws.onClose.listen((e) {
    print('Websocket closed, retrying in $retrySeconds seconds');
    scheduleReconnect();
  });

  ws.onError.listen((e) {
    print('Error connecting to ws');
    scheduleReconnect();
  });

  ws.onMessage.listen((MessageEvent e) {
    print('Received message: ${e.data}');
  });
}
```

### 更多信息 More information 

This section barely scratched the surface of using the dart:html library. For more information, see the documentation for [dart:html.](https://api.dart.dev/stable/dart-html/dart-html-library.html) Dart has additional libraries for more specialized web APIs, such as [web audio,](https://api.dart.dev/stable/dart-web_audio/dart-web_audio-library.html) [IndexedDB,](https://api.dart.dev/stable/dart-indexed_db/dart-indexed_db-library.html) and [WebGL.](https://api.dart.dev/stable/dart-web_gl/dart-web_gl-library.html)

​	本节仅简单介绍了 dart:html 库的使用。有关更多信息，请参阅 dart:html 的文档。Dart 还有其他库可用于更专业的 Web API，例如 Web 音频、IndexedDB 和 WebGL。

For more information about Dart web libraries, see the [web library overview.](https://dart.dev/web/libraries)

​	有关 Dart Web 库的更多信息，请参阅 Web 库概述。
