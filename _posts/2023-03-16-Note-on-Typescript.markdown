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


# Index Signatures & keyof Assertions

// Index Signatures

interface TransactionObj {
  Pizza: number,
  Books: number,
  Job: number
}

const todaysTransactions: TransactionObj = {
  Pizza: -10,
  Books: -5,
  Job: 50
}

interface TransactionObj {
  [index: string]: number
}

////////////////

interface Student {
  [key: string]: string | number | number[] | undefined,
  name: string,
  GPA: number,
  classes?: number[]
}

const student: Student = {
  name: "Dong",
  GPA: 3.5,
  classes: [100, 200] 
}

for(const key in student) {
  console.log(`${key}: ${student[key as keyof Student]}`)
}

type Stream = 'salary' | 'bouns' | 'sidehustle'

type Incomes = Record<Streams, number | string>

const monthlyIncomes: Incomes = {
  salary: 500,
  bonus: 100,
  sidehustle: 250
}

for(const revenue in monthlyIncomes) {
  console.log(monthlyIncomes[revenue as keyof Incomes])
}

# Generics

const echo = <T>(arg: T): T => arg
const isObj = <T>(arg: T): boolean => {
  return (typeof arg === 'obj' && !Array.isArray(arg) && arg !== null)
}

const isTrue = <T>(arg: T): { arg: T, is: boolean} => {
  if(Array.isArray(arg) && !arg.length) {
    return {arg, is: false}
  }
  if(isObj(arg) && !Object.keys(arg as keyof T).length) {
    return {art, is: false}
  }
  return {arg, is: !!arg}

}

# Utility Types

// Partial 

interface Assignment {
    studentId: string,
    title: string,
    grade: number,
    verified?: boolean,
}

const updateAssignment = (assign: Assignment, propsToUpdate: Partial<Assignment>): Assignment => {
    return { ...assign, ...propsToUpdate }
}

const assign1: Assignment = {
    studentId: "compsci123",
    title: "Final Project",
    grade: 0,
}

console.log(updateAssignment(assign1, { grade: 95 }))
const assignGraded: Assignment = updateAssignment(assign1, { grade: 95 })


// Required and Readonly 

const recordAssignment = (assign: Required<Assignment>): Assignment => {
    // send to database, etc. 
    return assign
}

const assignVerified: Readonly<Assignment> = { ...assignGraded, verified: true }

recordAssignment({ ...assignGraded, verified: true })


// Record 
const hexColorMap: Record<string, string> = {
    red: "FF0000",
    green: "00FF00",
    blue: "0000FF",
}

// Pick and Omit 

type AssignResult = Pick<Assignment, "studentId" | "grade">

const score: AssignResult = {
    studentId: "k123",
    grade: 85,
}

type AssignPreview = Omit<Assignment, "grade" | "verified">

const preview: AssignPreview = {
    studentId: "k123",
    title: "Final Project",
}


// Exclude and Extract 

type adjustedGrade = Exclude<LetterGrades, "U">

type highGrades = Extract<LetterGrades, "A" | "B">


// Nonnullable 

type AllPossibleGrades = 'Dave' | 'John' | null | undefined
type NamesOnly = NonNullable<AllPossibleGrades>

// ReturnType 


const createNewAssign = (title: string, points: number) => {
    return { title, points }
}

type NewAssign = ReturnType<typeof createNewAssign>

const tsAssign: NewAssign = createNewAssign("Utility Types", 100)

// Parameters 

type AssignParams = Parameters<typeof createNewAssign>

const assignArgs: AssignParams = ["Generics", 100]

const tsAssign2: NewAssign = createNewAssign(...assignArgs)




// Awaited - helps us with the ReturnType of a Promise 

interface User {
    id: number,
    name: string,
    username: string,
    email: string,
}

const fetchUsers = async (): Promise<User[]> => {

    const data = await fetch(
        'https://jsonplaceholder.typicode.com/users'
    ).then(res => {
        return res.json()
    }).catch(err => {
        if (err instanceof Error) console.log(err.message)
    })
    return data
}

type FetchUsersReturnType = Awaited<ReturnType<typeof fetchUsers>>



