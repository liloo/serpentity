# Serpentity

Serpentity is a simple entity framework inspired by [Ash][ash].

Usage:

    require('serpentity');

## Instantiating an engine

    var engine = Serpentity();

Add entities or systems, systems are added with a priority (the smaller
the number, the earlier it will be called):

    engine.addEntity(entityFactory());
    engine.addSystem(new GameSystem(), priority);

Update all systems:

    engine.update(dt);

Remove entities or systems:

    engine.removeEntity(entityReference);
    engine.removeSystem(systemReference);

## Creating Entities

Entities are the basic object of Serpentity, and they do nothing.

    var entity = new Serpentity.Entity();

All the behavior is added through components

## Creating Components

Components define data that we can add to an entity. This data will
eventually be consumed by "Systems"

    Class("PositionComponent").inherits(Serpentity.Component)({
        prototype : {
            x : 0,
            y : 0
        }
    });

You can add components to entities by using the addComponent method:

    entity.addComponent(new PositionComponent());


Systems can refer to entities by requesting nodes.

## Working with Nodes

Nodes are sets of components that you define, so your system can require
entities that always follow the API defined in the node.

    Class("MovementNode").inherits(Serpentity.Node)({
        types : {
            position : PositionComponent,
            motion : MotionComponent
        }
    });

You can then request an array of all the nodes representing entities
that comply with that API

    engine.getNodes(MovementNode);

## Creating Systems

Systems are called on every update, and they use components through nodes.

    Class("TestSystem").inherits(Serpentity.System)({
        prototype : {
            added : function added(engine){
                this.nodeList = engine.getNodes(MovementNode);
            },
            removed : function removed(engine){
                this.nodeList = undefined;
            }
            update : function update(dt){
                this.nodeList.forEach(function (node) {
                    console.log("Current position is: " + node.position.x + "," + node.position.y);
                });
            }
        }
    });

## That's it

Just run `engine.update(dt)` in your game loop :D

## Checking it in the frontend (dev).

You can link the bower package to test it out locally.
Spawn a python server (`python -m SimpleHTTPServer`) to see
the test page in `http://localhost:8000/browser_test/`


## TO-DO

* Removing components
* Implement the ashteroids demo (Serpentoids)
* Actually check performance

[ash]: http://www.ashframework.org/
