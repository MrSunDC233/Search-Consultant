# Search-Consultant

一个帮你更好地搜索的小项目。Something to help you search better instead of harder.
**🇨🇳 [中文](#总述) | 🇬🇧 [English](#overview)**

---

## 总述

Search Consultant（以下简称SC）是一个简单的Microsoft Edge浏览器插件。它的主要作用就是帮助你随时在几乎任何平台上更好地搜索。
从某种意义上说，这是一个AI Agent的载体，因为你需要一个外置的人工智能（Task-AI，即任务导向AI，以下简称T-A）来使项目的功能完整。

## 主要功能

SC内置的系统提示词结构化地拆解了在这一次人工智能浪潮前，人们是如何通过构建“搜索关键词组”，也就是一系列特定的关键词，来准确地在搜索平台上获取信息的。借助这样的提示词，T-A可以像这次人工智能浪潮来临前的信息检索高手一样帮助你依据需求写出高质量的搜索关键词组。额外的好处是，即使你尚且没有头绪，也可以提出问题，T-A会依循提示词的指令反问，并逐步帮你明确自己的需求。

## 特点

Edge的插件有一个令人痛苦的性质，一旦你不小心在popup之外点击或新窗口被打开，popup就会被关掉。考虑到本插件的用途，部分交互按键被设计为长按操作，这样可以防止误触导致打开新的窗口，关掉popup，然后你还得大费周章重新打开它。

除此之外，你可以自定义选项来配合你的搜索习惯。

- 主输入框：写你的主要问题。
- 副输入框：给你的问题定性（可以留空），帮助T-A更好地组织搜索关键词组。
- 关键词组数量：让T-A提供多少组关键词组，最少1组最多5组。
- 系统提示词：如果你有更好的方案来组织搜索关键词组，你可以自行修改或导入自己的提示词作为系统提示词。（规划中）
- 长按按键持续时长：想不想要试试持续长按50秒才能触发的按键？

---
> Translated from Chinese by myself. I'm expanding this README sentence by sentence—no guarantees on how natural it sounds. If a native speaker is reading this: I apologize in advance.

## Overview

Search Consultant (SC) is a simple Microsoft Edge extension, which can help you search better instead of harder. You can do it on almost any platform.
In a sense, it serves as an AI agent interface. You'll need to connect it to an external AI to use it. I call that AI a Task-AI (or T-A for short), since it's the one doing the heavy lifting.

## What can it do for you?

SC's built-in system prompt is engineered to teach T-A to construct a "search keyword group" (or more than one), which are series of specific keywords to retrieve information accurately on search platforms. It works just like those people who excelled in searching information on the internet in the pre-generative-AI era.

As a bonus, you can simply throw in a question even if you don't have any clear idea yet. Following the system prompt, T-A will ask you several questions to help you nail down what you actually want to find.

## Features

There's one frustrating quirk for all Edge extensions: the popup closes if you click outside the popup (maybe accidentally), or a new window is opened. So for buttons that open a new window, or critical ones like 'delete all search history on SC', a long-press action is required. This prevents accidental taps from ruining everything—including your day.

Plus, you can customize options to match your own searching habits:
- **Main Input**: Your core question.
- **Secondary Input**(optional): Allows you to add qualifiers to help T-A organize keyword groups better.
- **Keyword Group Amount**: How many keyword groups T-A should generate for you, from 1 to 5.
- **Custom System Prompt**: Replace the built-in system prompt with your own one to let T-A organize keywords in other ways. **(Planned)**
- **Long-Press Duration**: Fancy a button that needs a full 50-second press to trigger?
