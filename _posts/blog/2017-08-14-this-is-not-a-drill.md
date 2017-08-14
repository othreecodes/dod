---
layout: post
title: This is not a Drill
excerpt: "This marks the begining of a new Dev writer(sorta). After spending half my time trying to set this up, here it is."
categories: articles
tags: [first-post, code, type-script, Ionic]
comments: true
share: true
modified: 2017-08-14 16:25:00.820983
---

Sooo...
This is it. DoD. TH does that mean.. who knows, but it sounds cool.

While working on an Ionic project today (A game sorta app), [Firebase](http://firebase.google.com) errors started to kick in. After a couple minutes of debugging, found out i could'nt use *"this"* in an anonymous  javascript function. here's the snippet. 

```typescript

 GameStatus.on('value', function(snapshot) {
    console.log(snapshot.val())
    this.deck = snapshot.val().deck
    this.card = snapshot.val().initCard
    this.playerTwoCards = snapshot.val().joinCard
    this.lastCard = snapshot.val().lastcard
    this.isTurn = snapshot.val().status

    console.log(this.playerTwoCards);      
    console.log("Cards Updated")
    console.log('Local deck is '+deck)
    console.log('Global deck is '+this.deck)
      
    console.log(snapshot.val())
     }); 

```

The error was simple and confusing `this is null`. After a couple of minutes of googling, I found out that it had something to do with [Javascript Closures](https://stackoverflow.com/questions/111102/how-do-javascript-closures-work). So yeah, "this" was null. Then I Tried something funny. I don't know why I thought it would work.

```typescript
    GameStatus.on('value', function(snapshot) {
        console.log(snapshot.val())
       JoinedGame.prototype.deck = snapshot.val().deck
       JoinedGame.prototype.card = snapshot.val().initCard
       JoinedGame.prototype.playerTwoCards = snapshot.val().joinCard
       JoinedGame.prototype.lastCard = snapshot.val().lastcard
       JoinedGame.prototype.isTurn = snapshot.val().status

        console.log(JoinedGame.prototype.playerTwoCards);
        console.log("Cards Updated")
        console.log('Local deck is '+deck)
        console.log('Global deck is '+this.deck)
      
       console.log(snapshot.val())
     });

```

Of course, that would'nt work ...[What the heck does prototype do?](https://stackoverflow.com/questions/572897/how-does-javascript-prototype-work)

Eventually, I came up with this solution. The **Correct** solution by the way.

```typescript
    var _this = this;
    GameStatus.on('value', function(snapshot) {

      _this.deck = snapshot.val().deck
      _this.card = snapshot.val().joinCard
      _this.playerTwoCards = snapshot.val().initCard  
      _this.lastCard = snapshot.val().lastcard
      _this.isTurn = snapshot.val().status
      console.log("Cards Updated")
      console.log(snapshot.val())
      console.log(deck); //not null

    });

```

And the rest sorted Itself out. Off to more debugging now... I'll be updating this as more stuff happens.