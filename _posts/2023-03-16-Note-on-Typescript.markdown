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

# Classes

class Coder {
  constructor(
    public readonly name: string, 
    public music:string, 
    private age: number, 
    protected lang: string = 'Typescript'
  ) {
    this.name: name
    this.music: music
    this.age: age
    this.lang: lang
  }

  public getAge() {
    return `Hello, I'm ${this.age}`
  }
}

const Dave = new Coder('Dave', 'Rock', 32)


class WebDev extends Coder {
  constructor(
    public computer: string,
    name: string, 
    music:string, 
    age: number, 

  ) {
    super(name, music, age)
    this.computer = computer
  }

  public getLang() {
    return `I write ${this.lang}`
  }
}

const Sara = new WebDev('Mac', 'Sara', 'Lofi',25)
console.log(Sara.getLang()) // I write Typescript


interface Musician {
  name: string,
  instrument: string,
  play(action: string): string
}

class Guitarist implements Musician {
  name: string
  instrument: string

  constructor(name: string, instrument: string){
    this.name = name;
    this.instrument =instrument
  }

  play(action: string) {
    return `${this.name} ${action} the ${this.instrument}`
  }
}

const Page = new Guitarist('Jimmy', 'guitar')
conosle.log(Page.play('strums')) // Jimmy strums the guitar

class Peeps {
  static count: number = 0
  static getCount(): number {
    return Peeps.count
  }

  public id: number
  construtor(public name: string) {
    this.name = name
    this.id = ++Peeps.count
  }
}

const John = new Peeps('John')
const Steve = new Peeps('Steve')
const Amy = new Peeps('Amy')


class Bands {
  private dataState: string[]

  construtor() {
    this.dataState = []
  }

  public get data(): string[] {
    return this.dataState
  }

  public set data(value: string[]) {
    if(Array.isArray(value) && value.every(el=>typeof el === 'string')) {
      this.dateState = value
      return
    } else throw new Error('Param is not an array of strings')

  } 
}

const MyBands = new Bands()
MyBands.data = ['Neil Young', 'Led Zep']

console.log(MyBands.data) // ['Neil Young', 'Led Zep']
MyBands.data = [...MyBands.data, 'ZZ Top']
console.log(MyBands.data) // ['Neil Young', 'Led Zep','ZZ Top']