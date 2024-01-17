---
layout: post
title: Note on Typescript / Typescript 笔记
date: 2024-01-09 22:58
categories: [blog]
tags: [Typescript, Note]
description:
---

> TypeScript is a superset of JavaScript. As such, a JavaScript program is also a valid TypeScript program and a TypeScript program can seamlessly consume JavaScript.

<center><img src="/images/typescript/typescript.png" alt="typescript superset" width="80%" /></center>

TS: Strongly Typed Language; Statically typed Language (Checked at compile time)
JS: Loosely Typed Language; Dynamically typed Language (Checked at run time)


## Hello World

const hello : string = "Hello World!"

let a: string = 'abc'
let b: number = 123
let c: boolean = true
let d: any = ''
let re: RegExp = /\w+/g


const sum = (a: number, b: number) => {
    return a + b;
}

# Union type

let postId: string | number
let isActive: number | boolean


let re = /\w+/g

# Arrays & Objects

let stringArr = ['one','two','three'] // string
let guitars = ['one','two',123] // string | number
let mixed = ['one',1992,true] // string | number | boolean
let empty = [] // any

let bands: stirng[] = [];
 

// Tuple: exactly number, type and position
let myTuple: [stirng, number, boolean]

// Objects

let myObj: object

const exampleObj = {
  prop1: 'abc',
  prop2: true
}

const exampleObj: {
    prop1: string;
    prop2: boolean;
}

type Guitarist = {
  name: string,
  active: boolean,
  albums: (string | number)[]
}


let evh: Guitarist = {
  name: 'Chen',
  active: false,
  albums: [1992, 5140, 'Test']
}

// Enums

enum Grade {
  U, D, C, B, A
}

Grade.U // 0

# Functions



// Type Aliases

type stringOrNumber = string | number

type stringOrNumberArray = (string | number)[]

type Guitarist = {
    name?: string,
    active: boolean,
    albums: (string | number)[]
  }
  
type UserId = stringOrNumber

// Literal types

let myName: 'Chen'
let userName: 'Chen' | 'Li' | 'Peng'

// functions

const add = (a: number, b: number) : number => {
    return a + b;
}

const logMsg = (message: any): void => {
  console.log(message)
}

type mathFunction = (a: number, b: number) => number

let multiply: mathFunction = function (c,d) {
  return c * d
}

interface mathFunction {
  (a: number, b: number): number
}

// optional parameters
const addAll = (a:number, b:number, c?:number): number => {
  if(typeof c !== 'undefined') {
    return a + b + c
  }
  return a + b
}

// Rest parameters

cosnt total = (a: number, ...nums: number[]) : number => {
  return a + nums.reduce((prev, curr) => prev + curr)
}

# Assertions
