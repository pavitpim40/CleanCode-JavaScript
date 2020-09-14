# CleanCode-JavaScript

## Table of Contents

1. [Introduction](#introduction)
2. [Variables](#variables)
3. [Functions](#functions)
4. [Objects and Data Structures](#objects-and-data-structures)
5. [Classes](#classes)
6. [SOLID](#solid)
7. [Testing](#testing)
8. [Concurrency](#concurrency)
9. [Error Handling](#error-handling)
10. [Formatting](#formatting)
11. [Comments](#comments)
12. [Translation](#translation)

## Introduction

![Humorous image of software quality estimation as a count of how many expletives
you shout when reading code](https://www.osnews.com/images/comics/wtfm.jpg)

หลักการ Clean Code สร้างขึ้นจากพื้นฐาน 3R
1.Readable : อ่านทำความเข้าใจง่าย เช่น ชื่อตัวแปรสื่อความหมาย ,โค้ดมีรูปแบบเดียวกันสม่ำเสมอ
2.Resuable : สามารถใช้โค้ดซ้ำได้ เช่น ไม่ต้องเขียนโมดูลบางอย่าใหม่หมด , ฟังก์ชันมีขนาดเล็ก , มีการหลีกเลี่ยง Side effect
3.refactorable : สามารถแก้ไขแแยกส่วนโค้ดได้ โดยไม่ส่งผลกับโปรแกรมใหญ่ทั้งหมด


อีกสิ่งหนึ่งคืออย่าพยายามทำให้สมบูรณ์แบบตั้งแต่ในทีแรก
อย่าเร่งรีบกดเอาชนะตัวเองด้วยโค้ดที่สมบูรณ์แบบ เปลี่ยนมาเอาชนะโค้ดทีละเล็กทีละน้อยแทน