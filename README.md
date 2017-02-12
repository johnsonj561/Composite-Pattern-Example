----------------------------------------------------------------------------------------------------------------
Title: The Composite Pattern

Sources:
Notes below regarding composite pattern taken from "Design Patterns - Elements of Reusable Object-Oriented Software"
By Gamma, Helm, Johnson, Vlissides

Example code from Derek Banas tutorial
Video: https://www.youtube.com/watch?v=2HUnoKyC9l0
Source: http://www.newthinktank.com/2012/10/composite-design-pattern-tutorial/

Author: Justin J

Purpose: FAU Object Oriented Software Design Course, Sprint 2017

----------------------------------------------------------------------------------------------------------------

Intent
Compose objects into tree structures 
- represent part-whole hierarchies
- allow clients to treat objects and compositions uniformly

Motivation
- decrease complexity and client responsibility
- abstract class that represents primatives and their containers is key

Applicability
- to represent part-whole hierarchies of objects
- to allow clients to ignore difference between compositions of objects and individual objects

Structure
- see class diagram structure.di

Participants
- Component	- declares interface for objects in composition
           	- implements default behavior for interface common to all classes
            - declares interface for accessing and managing child components
            - optionally defines interface for accessing component parent
- Leaf      - has no children, defines behavior for primitive objects
- Composite - defines behavior for components with no children
          	- stores child components
          	- implement child related operations in Component interface
- Client	- maniuplates objects in composition through the Component interface

Collaborations
- Clients use component class interface to interact with objects in composite structure

Consequences
- defines class hierarchies consisting of primitive objects and composite objects
- primitive objects can be composed into more complex objects, which can again be composed recursively
- simplifies client interactions, clients don't know or care if they're dealing with a composite or primitive
- simplifies adding new components

Implementation
- explicit parent references
	- parent reference simplifies moving up the structure and deleting of component
	- define parent reference in component class, leaves and composites can inherit
	- maintain invariant that all children of a composite have as their parent the composite that in turn has
	  them as their children. Only change parent when it's being added/removed from composite. Try to implement
	  this once in the composite class so it is inherited and invariant maintained
- sharing components
	- sharing components can reduce storage requirements
	- be careful and refer to flyweight pattern 
- Maximize component interface
	- allow component interface to define as many common operations as possible
	- component class defines default operations, subclasses can then override
- Declare child management operations - Should we declare add/remove operations in Component or Composite?
	- there is tradeoff between safety and transparency
	- defining at root ((component) allows transparency but disadvantage is cients can then try to add/remove from leaves
	- defining at composite gives safety by not allowing clients to add/remove from leaves, caught at compile time
	  this loses transparency, leaves and composites have different interfaces 
	- typically best to make add/remove fail by default (exception) if component isn't allowed to have children
- Should component implement a list of components?
 	- putting child pointer in base class incurs space penalty for every leaf
 	- only do this if there are few children
 - Child ordering
 	- must design child access and management interfaces carefully when child ordering is required, must maintin sequence
 - Caching to improve performance
 	- if you need to search compositions frequently, composite class can cache traversal or search info about children
 	- composite can cache actual results or information to allow short-circuit traversal/search
 	- changes to a component will force invalidating caches of parent. This works bets when components know their parents
 - Who should delete components?
 	- if no garbage collection, best to make Composite responsible for deleting children.
 	- exception - if leaf objects are immutable
 - Best Data Structure for storing components?
 	- linked lists, trees, arrays, hash tables are all options
 	- choice depends on efficiency and trade offs
 
 Related Patterns
 - component-parent link is used for a Chane of Responsibility
 - Decorator often used with Composite
 - Flyweight lets you share components, but can no longer refer to their parent
 - Iterator can be used to traverse composites
 - Visitor localizes operations and behavior that would otherwise be distributed across composite and leave classes
 

          	