# Collision Query Protocol

CQP/March 2025; Igni Project, Draft 1

This document is an informal draft. It may not be entirely unambiguous, grammatically correct or consistent. The document, as it stands, merely exists to assist development of a more refined document later on.

## Table of Contents
1. Terminology
2. Introduction
3. Requests
    1. Cast Point
    2. Cast Ray
4. Authors Notes

## 1. Terminology

This section defines various words and abbreviations used throughout this document.

**Float**
- Floating-point integer; a binary format that supports rational values

**Hitbox**
- Invisible object, of any shape, used for collision detection

**Request**
- Message sent from a client-to-server, further defined in section 2.


## 2. Introduction
The Collision Query Protocol (CQP) is an application-layer communication protocol built to allow interactivity between processes in a 3-dimensional space. 

## 3. Requests

| Size (bytes)	| Value	        | Description   |
|---------------|---------------|---------------|
| 1	            | 8-bit integer	| Request code  |
| 0+	        | Raw data	    | Request data  |

A request is a message sent from a client to a CQP server. Requests consist of an 8-bit request code followed by the requests data, which varies in size.

Various types of requests are assigned unique numbers. From these numbers, a program reading a request may extrapolate the purpose of the request and structure of the requests contents. This number is referred to as a ‘request code’ and is 8 bits in size.

## 3.1. Cast Point

The ‘cast point’ request queries whether a point at a given location intersects a hitbox. This request has a code of 0.

| Size (bytes)	| Value	        | Description   |
|---------------|---------------|---------------|
| 4             | float         | X location    |
| 4             | float         | Y location    |
| 4             | float         | Z location    |

The ‘cast point’ request consists of 3 floating-point integers: the X, Y and Z location of the point to be cast, in that order.

## 3.2. Cast Ray

A ‘cast ray’ request queries whether a straight, infinite line of specified origin and vector intersects a hitbox. This request has a code of 1.

| Size (bytes)	| Value	        | Description   |
|---------------|---------------|---------------|
| 4             | float         | X origin      |
| 4             | float         | Y origin      |
| 4             | float         | Z origin      |
| 4             | float         | X vector      |
| 4             | float         | Y vector      |
| 4             | float         | Z vector      |

The ‘cast ray’ request consists of 2 sets of floating-point X-Y-Z coordinates: the origin of the ray, followed by its vector. Both sets of coordinates are 3-dimensional, and are ordered X first, Y second and Z third.

## 4. Authors Notes
Originally titled ‘Hitbox Update Protocol’, the protocol has recently gone through an overhaul that substantially simplified its design.


Previously, a central server would hold all hitboxes and process all collisions between them. Clients would send requests to the server to modify hitboxes, in a similar fashion to the Scene Update Protocol. 

The redesign, however, places the tasks of storing hitboxes and computing collisions on the clients. All a CQP server does is forward each message it receives to all its clients.


