---
title: React Autocomplete（自动补全）组件
components: TextField, Popper, Autocomplete
githubLabel: 'component: Autocomplete'
waiAria: 'https://www.w3.org/TR/wai-aria-practices/#combobox'
---

# Autocomplete 自动补全组件

<p class="description">自动补全是一个普通文本输入框，它通过一组建议的选项来帮助用户输入。</p>

该组件常用于以下两个场景中的单行文本框赋值：

1. 文本框必须取值于某个预设好的，例如：一个位置域必须包含一个有效的位置名称： [组合框](#combo-box)。
2. 文本框也可以是任何值，但最好能够为用户提供可能的选项，譬如搜索框可以提供近似的或者曾搜索过的选项以节省用户时间：[灵活的单文本框](#free-solo)。

此组件旨在改进 “react-select” 和 “downshift” 这两个包。

{{"component": "modules/components/ComponentLinkHeader.js"}}

## Combo box 组合框

必须取值于一个预设的可选数据源。

{{"demo": "pages/components/autocomplete/ComboBox.js"}}

### 选项结构

默认情况下，该组件接受和以下结构相同的选项：

```ts
interface AutocompleteOption {
  label: string;
}
// 或者
type AutocompleteOption = string;
```

例如：

```js
const options = [
  { label: 'The Godfather', id: 1 },
  { label: 'Pulp Fiction', id: 2 },
];
// or
const options = ['The Godfather', 'Pulp Fiction'];
```

然而，你也可以通过提供 `getOptionLabel` 属性来使用不同的结构。

### 练习

下面的每个示例都是自动完成组件的一个功能点的演示。

{{"demo": "pages/components/autocomplete/Playground.js"}}

### 选择一个国家

从248个国家中选择一个。

{{"demo": "pages/components/autocomplete/CountrySelect.js"}}

### 可控的状态

此组件有两种可控的状态：

1. “value” 状态（state）包含了 `value`/`onChange` 两种属性的组合。 这个状态表示用户选择的值，如当按下 <kbd class="key">Enter</kbd> 键时。
2. “input value” 状态（state) 则包含了 `inputValue`/`onInputChange` 两种属性的组合。 这个状态展示了在文本框中显示的值。

> ⚠️ 以上两种状态互不干涉，它们应该被单独控制着。

{{"demo": "pages/components/autocomplete/ControllableStates.js"}}

## Free solo

当将 `freeSolo` 设置为 true 时，用户可以文本框中输入任意值。

### 搜索输入栏

该属性的主要使用方式是创建一个带有搜索建议的 **输入文本框**，例如 Google 搜索 或 react-autowhatever。

{{"demo": "pages/components/autocomplete/FreeSolo.js"}}

### Creatable （可创造性）

如果您打算将此模块用于类似 [组合框](#combo-box) 的体验（一个选择控件元素的增强版），我们则建议如下的设置：

- `selectOnFocus` to help the user clear the selected value.
- `clearOnBlur` to help the user enter a new value.
- `handleHomeEndKeys` 使用<kbd class="key">Home</kbd> 和 <kbd class="key">End</kbd> 键在弹出窗口内移动焦点。
- 最后一个选项，例如 `加上 "你的搜索结果"`。

{{"demo": "pages/components/autocomplete/FreeSoloCreateOption.js"}}

您也可以在用户想要加入一个新值的时候显示一个对话框。

{{"demo": "pages/components/autocomplete/FreeSoloCreateOptionDialog.js"}}

## 分组

你可以使用 `groupBy` 属性对选项进行分组。 如果你要这样做，请先确保选项也按照它们分组的相同维度进行排序， 否则你将会注意到重复的标题。

{{"demo": "pages/components/autocomplete/Grouped.js"}}

## 失效的选项

{{"demo": "pages/components/autocomplete/DisabledOptions.js"}}

## `useAutocomplete`

For advanced customization use cases, a headless `useAutocomplete()` hook is exposed. 它接受几乎与 Autocomplete 组件相同的参数，辅以与 JSX 渲染有关的所有参数。 自动完成组件是建立在这个钩子上。

```tsx
import { useAutocomplete } from '@mui/base/AutocompleteUnstyled';
```

The `useAutocomplete` hook is also reexported from @mui/material for convenience and backward compatibility.

```tsx
import useAutocomplete from '@mui/material/useAutocomplete';
```

- 📦  [4.5kB 的压缩包](/size-snapshot)。

{{"demo": "pages/components/autocomplete/UseAutocomplete.js", "defaultCodeOpen": false}}

### 自定义的 hook

{{"demo": "pages/components/autocomplete/CustomizedHook.js"}}

Head to the [customization](#customization) section for an example with the `Autocomplete` component instead of the hook.

## 异步请求

The component supports two different asynchronous use-cases:

- [打开时加载](#load-on-open)：它将等待用户与组件进行交互以加载选项。
- [当你键入内容时进行搜索](#search-as-you-type)：每一次键入都会提交一个新的请求。

### 打开时加载

It displays a progress state as long as the network request is pending.

{{"demo": "pages/components/autocomplete/Asynchronous.js"}}

### 当你键入内容时进行搜索

如果您的逻辑在每次按键时都要获取新的选项，并使用文本框的当前值在服务器上进行过滤，那么您可能需要考虑对请求进行节流处理。

此外，您需要通过 `filterOptions` 属性来禁用内置的 `Autocomplete（自动完成）` 组件的筛选功能：

```jsx
<Autocomplete filterOptions={(x) => x} />
```

### Google Maps Places

一个自定义 UI 来配合 Google 地图位置的自动完成功能。

{{"demo": "pages/components/autocomplete/GoogleMaps.js"}}

在这个演示中，我们需要加载 [Google Maps JavaScript](https://developers.google.com/maps/documentation/javascript/tutorial) API。

> ⚠️在你开始使用 Google Maps JavaScript API 之前，你必须注册并且创建一个可支付的账户。

## 多个值

当然您也可以将其作为标签，这样用户就可以输入更多的值。

{{"demo": "pages/components/autocomplete/Tags.js"}}

### 固定的选项

有时候您需要锁定某个标签，这样他们不会被从界面中移除，那么这时您可以将 chips 设置为禁用来达到这个目的。

{{"demo": "pages/components/autocomplete/FixedTags.js"}}

### Checkboxes 复选框

{{"demo": "pages/components/autocomplete/CheckboxesTags.js"}}

### 限制标签数量

当没有聚焦时，你可以使用 `limitTags` 属性来限制显示选项的数量。

{{"demo": "pages/components/autocomplete/LimitTags.js"}}

## 尺寸

想要使用外观看起来比较小的输入框吗？ 试着使用 `size` 属性吧。

{{"demo": "pages/components/autocomplete/Sizes.js"}}

## Customization 个性化

### 自定义输入

使用 `renderInput` 属性，您可以对输入内容进行自定义渲染。 此 render 属性的第一个参数包含了您想要传递的那些属性。 请特别注意 `ref` 和 `inputProps` 。

{{"demo": "pages/components/autocomplete/CustomInputAutocomplete.js"}}

### GitHub 标签选择器

该演示再现了 GitHub 的标签选择器：

{{"demo": "pages/components/autocomplete/GitHubLabel.js"}}

在该示例中使用  `useAutocomplete`  hook 而不是组件，您也可以查看 [Customized hook](#customized-hook) 部分来了解自定义示例。

## 高亮显示

下面的演示依赖于 [autosuggest-highlight](https://github.com/moroshko/autosuggest-highlight)，这是极小的 (1 kB) 工具集，它用于突出显示自动建议和自动完成组件中的文本。

{{"demo": "pages/components/autocomplete/Highlights.js"}}

## 自定义筛选

此组件提供了一个 factory 来构建一个筛选的方法，来供给 `filterOptions` 属性使用。 你可以使用该方法来更改默认的筛选行为。

```js
import { createFilterOptions } from '@mui/material/Autocomplete';
```

### `createFilterOptions(config) => filterOptions`

#### 参数

1. `config` (_object_ [optional]):

- `config.ignoreAccents` (_bool_ [optional])：默认为  `true`。 移除字母的变音符号。
- `config.ignoreCase` (_bool_ [optional])：默认为 `true`。 所有字母都小写。
- `config.limit` (*number* [optional]): 默认值为 null。 显示限定数量的建议选项。 例如，如果 `config.limit` 是 `100`,，那么只显示前 `100 个` 匹配的选项。 如果存在很多选项匹配，并且虚拟化设置还没建立成时，这样的限制是非常有效的。
- `config.matchFrom` (_'any' | 'start'_ [optional]): 默认值为 `'any'`。
- `config.stringify` (*func* [optional]): 控制如何将一个选项转换成一个字符串，这样，选项就能够和输入文本的片段相匹配。
- `config.trim` (_bool_ [optional])：默认为 `false`。 删除尾随空格。

#### 返回结果

`filterOptions`：返回的 filter（过滤）方法可以直接提供给带有 `filterOptions` 属性的 `Autocomplete` 组件，或者和 hooks 同名的参数。

在下面的演示中，选项需要以查询前缀开始：

```jsx
const filterOptions = createFilterOptions({
  matchFrom: 'start',
  stringify: (option) => option.title,
});

<Autocomplete filterOptions={filterOptions} />;
```

{{"demo": "pages/components/autocomplete/Filter.js", "defaultCodeOpen": false}}

### Advanced 进阶

对于更复杂的过滤机制，譬如模糊匹配（fuzzy matching），我们推荐您看一下 [match-sorter](https://github.com/kentcdodds/match-sorter)。 就像这样：

```jsx
import { matchSorter } from 'match-sorter';

const filterOptions = (options, { inputValue }) => matchSorter(options, inputValue);

<Autocomplete filterOptions={filterOptions} />;
```

## 虚拟滚动

在 10000 个随机生成的选项中搜索。 多亏了 [react-window](https://github.com/bvaughn/react-window)，这个列表得以实现虚拟滚动。

{{"demo": "pages/components/autocomplete/Virtualize.js"}}

## 事件

如果您想要阻止默认的按键行为，您可以将事件的 `defaultMuiPrevented` 属性设置为 `true`：

```jsx
<Autocomplete
  onKeyDown={(event) => {
    if (event.key === 'Enter') {
      // 阻止默认的 'Enter' 行为.
      event.defaultMuiPrevented = true;
      // 你的处理程序代码
    }
  }}
/>
```

## 设计局限

### autocomplete/autofill

Browsers have heuristics to help the user fill in form inputs. However, this can harm the UX of the component.

By default, the component disables the input **autocomplete** feature (remembering what the user has typed for a given field in a previous session) with the `autoComplete="off"` attribute. Google Chrome does not currently support this attribute setting ([Issue 587466](https://bugs.chromium.org/p/chromium/issues/detail?id=587466)). A possible workaround is to remove the `id` to have the component generate a random one.

In addition to remembering past entered values, the browser might also propose **autofill** suggestions (saved login, address, or payment details). 若您不需要自动填充，您可以尝试以下的方式：

- 给输入框一个不同的名字，这样不会给浏览器泄露任何可以滥用的信息。 例如：`id="field1"` 而不是 `id="country"`。 若你不填写 id 的话，该组件则会使用一个随机的 id。
- 设置 `autoComplete="new-password"`（当设置此属性时，有些浏览器会建议输入高复杂度的密码）。

  ```jsx
  <TextField
    {...params}
    inputProps={{
      ...params.inputProps,
      autoComplete: 'new-password',
    }}
  />
  ```

Read [the guide on MDN](https://developer.mozilla.org/en-US/docs/Web/Security/Securing_your_site/Turning_off_form_autocompletion) for more details.

### iOS VoiceOver 辅助功能

iOS Safari 中的 VoiceOver 对 `aria-owns` 属性的支持并不是很到位。 你可以用 `disablePortal` 属性来解决这个问题。

### ListboxComponent

若你提供一共自定义的 `ListboxComponent` 属性，请保证需要滚动功能的容器将 `role` 属性设置为 `listbox`。 这能保证滚动功能在一些情况下，例如当用键盘切换的时候，仍然能够正常显示。

## 无障碍设计

(WAI-ARIA: https://www.w3.org/TR/wai-aria-practices/#combobox)

我们鼓励用户在 textbox 中使用标签。 组件带入了 WAI-ARIA 授权的一些标准。
