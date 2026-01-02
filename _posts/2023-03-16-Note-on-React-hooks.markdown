---
layout: post
title: Note on React Hooks / React Hooks 笔记
date: 2024-03-10 14:11
categories: [blog]
tags: [React, Note]
description:
---


UseReducer

function reducer(state, action) {
  switch(action.type) {
```
case 'increment':
  return { count: state.count + 1 }
case 'decrement':
  return { count: state.count - 1 }
default:
  return state;
```
  }
}

const [state, dispatch] = useReducer(reducer, {count:0})

function increment() {
  dispatch()
}