# Introduction & Goals
Angular is black magic, and we want to make it easier to understand and learn. In this lab, you are going to create a Kaogotchi game in Service Portal. A **Kaogotchi** is a term that Maria Gabriela made up that combines two of her favorite things: A **Kaomoji** and a **Tamagotchi**. For those of you who may not be familiar with them, **Kaomojis** are a popular style of Japanese emoticon made up of Japanese characters and grammar punctuations and are used to express emotion in texting and cyber communications. (Source: <http://kaomoji.ru/en/>) 

Some fun examples of Kaomoji are:
- A really angry dude (ノ°益°)ノ
- A super happy bro ٩(◕‿◕｡)۶
- Me diligently writing this lab guide ....φ(・∀・\*)

On the other hand, **Tamagotchi** are handheld digital pets created in Japan that were a big hit in the 90s. (Source: <https://en.wikipedia.org/wiki/Tamagotchi>) You would carry around this little egg pet, and it would scream at you to clean up its poop and be fed. I was obsessed with them as a child. 

So today, we are making a Tamagotchi game with a cute little Kaomoji character in ServiceNow’s Service Portal! 

Much credit goes to the Codepen from which the original Tamagotchi code was obtained: <https://codepen.io/Creasium/pen/NWGOGrr>. You should check out other stuff on Codepen - people post some awesome things on there that they make happen with CSS that’s easy to port into ServicePortal to make your work look super cool. 

## What is an Angular Provider?
So, think of Angular providers in ServiceNow as a way for your computer to remember how to find something it needs.

Let's say you have a toy box full of toys and want to play with your favorite toy car. But you don't know where it is in the box! So, you ask your mom, "Mom, where is my favorite toy car?"

Your mom is like an Angular provider. She knows where to find your favorite toy car, so she tells you, "It's in the bottom left corner of the toy box."

In the same way, when you use Angular providers in ServiceNow, you're telling your computer where to find the things it needs to run your program. By creating a provider, you're giving your computer a "map" to find the proper objects and functions.

In ServiceNow Service Portal, Angular providers are objects that help you manage and organize dependencies and services used in your widgets and pages.

Think of a provider as a factory that creates and manages objects or services in your Service Portal. The provider is responsible for creating and configuring the object or service and making it available to other widgets and pages.

For example, if you have a service that needs to be used in multiple widgets or pages throughout your Service Portal, you can define it as a provider. The provider will then be responsible for creating and configuring the service and making it available to the other widgets and pages that need it.

Using Angular providers in ServiceNow Service Portal allows you to manage the lifecycle of objects and services in your Service Portal and ensure they are available where and when needed. This can help improve the performance and maintainability of your Service Portal by reducing duplication and enhancing code organization.

## Benefits of using an Angular Provider
There are several benefits to using Angular providers in ServiceNow. Here are some of the key benefits:

1. Data persistence and sharing: Data stored in Angular providers is persistent and can be accessed by other widgets that have the provider attached and called. This means that you don't have to make multiple server queries for the same data and can make your page load faster by sharing the information through the provide
2. Reusability: Logic stored in Angular providers is reusable by other widgets. This saves effort by not needing to maintain logic in multiple places
3. Simplified event handling: Using $broadcast and $emit to handle events requires all widgets to be in the correct scope, loaded, and actively listening. With an Angular provider, you don't have to worry about any other widgets listening in. When you update the provider, you don't have to change event names in multiple widgets. This makes it easier to manage event handling and reduces the risk of naming conflicts.
4. Improved performance and maintainability: Using Angular providers in ServiceNow can help improve the performance and maintainability of your application. By reducing duplication and improving code organization, Angular providers can help make your application easier to understand, debug, and maintain over time.

Overall, using Angular providers in ServiceNow can help make your application more efficient, easier to maintain, and more flexible in terms of data sharing and event handling.

## Lab Setup
The central hub for this lab will be this page: `[instancename].lab.service-now.com/kao.` Replace the `[instancename]` section with the name of your lab instance. It should be `CCL1256-DATE-NUMBER-NUMBER`. There will also be a link at the top of the filter navigator under the `All` menu, should you need it.

Throughout the Lab, I'll be referencing this landing page as a place you should use to navigate the exercises. The widgets you'll need are all already created and ready for convenience. I've added the CSS, so you don't have to worry about that, and you'll have a new widget at the start of each exercise, to avoid any rework in case stuff doesn't work at the end. 

You can look at the Example Widgets we worked on throughout building this lab to see the steps we took along the way. **Kaogotchi v0** is constructed mainly using broadcasts, event listeners, and the DOM and was made to be used outside of ServiceNow, so it looks wonky since it's ECMAScript 2021. **Kaogotchi v1** is where Eric started to work on the widget, and you can see its evolution as he made it into a proper ServiceNow widget through versions 2 and 3. 

# Exercise 0: Poke the Kaogotchi

## Goal
Let's play with the finished app and see how things work before we get started. 

## Instructions
Take a few minutes to navigate to the landing portal page for this lab at `[instancename].lab.service-now.com/kao.` Go to the `Exercise 0` link to see what this lab's finished product will look like. Once you click **Start**, enter a name for your Kaogotchi and watch the numbers tick! Click on the three buttons **Sleep**, **Feed**, and **Play** to keep your Kaogotchi alive and see how long it survives. That’s it! That’s the basics of the game. 

## Summary
So that’s what the basic game we will be building does. Let’s get started!

# Exercise 1: Build the HTML side of the widget
## Goal
We’re going to start building the HTML of the widget. This side of the code will remain the same throughout the lab, but we will go piece by piece to explain all the Angular elements in the HTML. 

Something to note is that, since we are working on pasting the code for the widget section by section, you will not have a "working" widget you can play with until the end of Lab 4. So at the start, all you'll see is just a gray box with the name of the widget we are editing inside.

## Instructions
 1. **Very Important:** When you first logged in to your instance, you were put on the landing page in the Next Experience UI. In the top right corner, click on the little globe icon and change to the Application Scope named "Kaogotchi". Let your tab refresh, and go from here. This way you'll avoid any out-of-scope issues in the rest of the lab.
 2. Go back to the landing page by following the link in the top right or going to `[instancename].lab.service-now.com/kao` and click "I'm ready to start Exercise 1". In the center of your screen, you should see a gray box with the text `Widget Name: Kaogotchi EX1 Widget`. This is the blank template for the widget you will be building; you can `CTRL+Right Click` on Mac and Windows inside the gray box, then click **Widget in Editor** to open up the **Widget Editor** in a new tab. Saving your work as you go is an excellent habit to get into, so I'd suggest you hit `ALT+S` every so often.
 3. We'll be adding HTML block by block into the `HTML Template` tab of the **Widget Editor**. This block is going to be the first page of our application. It has a title, a subheading, and three buttons - `Start`, `Settings`, and `Resume`.  
 ```html
<!-- Exercise 1.2 --> 
<div ng-init="c.checkResume()"
     class="the-box">
  Widget Name: Kaogotchi EX1 Widget

	<div ng-init="c.data.settings == 'main'" ng-if="c.data.settings == 'main'"
       class="main-menu-screen" >
    <div class="menu">
      <h1>Kaogotchi</h1>
      <h3>Welcome to my little Kaogotchi game</h3>
      <div class="menu-buttons">
        <button ng-click="c.startGame()" 
                name="start" 
                id="action-menu-start-game" 
                class="btn buttons btn-primary">Start</button>
        <button ng-click="c.settingsMenu()" 
                name="settings" 
                id="action-menu-settings" 
                class="btn btn-primary">Settings</button>
      </div>
      <div class="menu">
        <button ng-if="c.data.resume" ng-click="c.resumeGame()" 
                name="resume" 
                id="action-menu-resume" 
                class="btn btn-primary">Resume <br>
          {{c.data.name}} - {{c.data.scoreBar}} pt</button>
      </div>
    </div>
  </div>
```
3. Lines 2, 6, and 12 contain our first Angular Directives. As you navigate through this PDF, you'll see the Angular Directives most often highlighted in red on the first line of the HTML element they belong to. 
	 1. `ngInit` is used as a starting value to default to. In this case, we are saying that we will start with `c.data.settings == 'main'` then immediately saying that if `ng-if="c.data.settings =='main'"` then we want this div to appear. For more information, go to the [official AngularJS Docs for ngInit.](https://docs.angularjs.org/api/ng/directive/ngInit)
	 2. `ngIf` removes or recreates a portion of the DOM tree based on an {expression}. For more information, go to the [official AngularJS Docs for ngIf.](https://docs.angularjs.org/api/ng/directive/ngIf)
	 3. `ngClick` directive allows you to specify custom behavior when an element is clicked. For more information, go to the [official AngularJS Docs for ngClick](https://docs.angularjs.org/api/ng/directive/ngClick)
 4. Here's the next block of HTML - This is for the Settings page. We started with another `ngIf` to show this div if `c.data.settings =='settings'`. Here we find a new angular directive, `ngBind`.
```html
<!-- Exercise 1.4 --> 
  <div ng-if="c.data.settings == 'settings'"
       class="menu-screen-settings">
    <div class="settings">
      <h2>Settings</h2>
      <div class="settings-buttons">
        <h3>Difficulty: <span ng-bind="c.data.diff"></span></h3>
        <button ng-click="c.setDif('Hard')" 
                name="hard" 
                id="action-settings-difficulty-hard" 
                class="btn btn-danger">Hard</button>
        <button ng-click="c.setDif('Normal')" 
                name="normal" 
                id="action-settings-difficulty-normal" 
                class="btn btn-warning">Normal</button>
        <button ng-click="c.setDif('Easy')" 
                name="easy" 
                id="action-settings-difficulty-easy" 
                class="btn btn-success">Easy</button>
        <br>
        <button ng-click="c.mainMenu()" 
                name="mainMenu" 
                id="action-settings-back" 
                class="btn btn-primary">Back</button>
      </div>
    </div>
  </div>
```
5. With this block of HTML, we are creating the Settings page containing the difficulty buttons. If you click on any of the buttons, you'll see the Difficulty section automatically update. This is one of the awesome things about Angular - it all updates together simultaneously. The buttons will only work once we add our Client Script in Exercise 2, but it's good to familiarize yourself with what functions are being called in the `ngClick`s so you'll recognize them again in the next exercise.
	1. `ngBind` means you are binding data to an element. You are having the text of the element show whatever value you are passing to the variable. Instead of being interactive like a `ngClick`, `ngBind` shows the value and updates when that value changes. For more information, go to the [official AngularJS Docs for ngBind.](https://docs.angularjs.org/api/ng/directive/ngBind)
6. This is the next block of HTML. This is the actual gameplay screen, and you can see `c.data.settings` now being set to the value of `play`. We are binding the values of `c.data.sleepHp`, `c.data.hungerHp`, and `c.data.playHp` to the `span` tags so they are displayed and updated in real-time, and we are doing the same with the score bar at the bottom. The rest of the code should look pretty similar to you as it largely repeats what we have already discussed. Please give it a quick look over to familiarize yourself with what's about to come, particularly paying attention to the `ngClick`s. 
```html
  <!-- Exercise 1.6 --> 
  <div ng-if="c.data.settings == 'play'"
       class="game-screen">
    <div class="tamagotchi">
      <div ng-if="!c.data.score"
           class="hp">
        <p>Sleep: <span ng-bind="c.data.sleepHp"></span>%</p>
        <p>Hunger: <span ng-bind="c.data.hungerHp"></span>%</p>
        <p>Play: <span ng-bind="c.data.playHp"></span>%</p>
        <p id="score-bar">Score: <span ng-bind="c.data.scoreBar"></span></p>
      </div>
      <p><span ng-bind="c.data.name"></span></p>
      <p id="t-body"><span ng-bind="c.data.kaomoji"></span></p>
      <div class="buttons" ng-if="!c.data.score">
        <button ng-click="c.actionSleep()" 
                name="sleep" 
                id="action-sleep" 
                class="btn btn-primary">Sleep</button>
        <button ng-click="c.actionEat()" 
                name="feed" 
                id="action-feed" 
                class="btn btn-primary">Feed</button>
        <button ng-click="c.actionPlay()" 
                name="play" 
                id="action-play" 
                class="btn btn-primary">Play</button>
      </div>
      <div ng-if="c.data.score" ng-bind="c.data.score"></div>
    </div>
  </div>
</div>
```

## Summary
Good job! We have so far created the HTML for our widget. 

**Visual Verification:** At the end of the exercise, if you refresh your portal page, you shouldn't see anything other than the text showing the widget name. This is because we are still missing a large part of the game's code, and you are ok to keep moving forward in the lab.

# Exercise 2: Build the Client side of the widget
## Goal
Now it's time for the meat of the widget! We'll be building the Client Script side of the Widget now and making the widget work little by little. Let's get started.

You can navigate to the page we'll be working from in one of two ways:
1. Head back to the Exercise 1 portal page you should have open (`kaogotchi_ex1_page`), then go ahead and click on the **Next Exercise** button at the bottom to navigate to the page you'll be working from. 
2. Go back to the landing page by following the link in the top right of the Exercise 1 portal page or going to `[instancename].lab.service-now.com/kao` and click on "I'm ready to start Exercise 2". 

 In the center of your screen, you should see a gray box with the text `Widget Name: Kaogotchi EX2 Widget`.  This is the template for the widget you will be building; you can `CTRL+Right Click` on Mac and Windows inside the gray box, then click on **Widget in Editor** to open up the **Widget Editor** on a new tab. Saving your work as you go is an excellent habit to get into, so I'd suggest you hit `ALT+S` every so often.

## Instructions
1. Let's get started with the first block of JavaScript we'll be pasting into the Client Controller. Lines 1-6 should look familiar to you if you have worked in widgets before, but something to note is we have added services into the function call. Among them is a reference to our Angular Provider, `KaogotchiService`. This is important as no matter how hard you try; if you don't call it here and make sure to attach the provider to your widget, you'll never get your widget to listen for it. We'll go over how to attach the provider when we make it in Exercise 4.
	1. I know this block looks like a lot, but it's mainly just declaring variables in lines 10-16, then setting up basic functions. Something important to note is line 7, where we give our `KaogotchiService` a neat little nickname `c.kaoServ` - this lets us call it and functions within it more easily. We'll be calling it multiple times throughout the Client Script. I've also added several comments to specify what the code is doing for future reference.
```javascript
api.controller = function($interval, spModal, KaogotchiService) {
	
	/* Exercise 2.1 */  

	/* widget controller */
	var c = this;
	c.kaoServ = KaogotchiService; //gives the Angular Provider Service a more friendly local name
	c.data.settings = c.kaoServ.getSetting();

	var tmgch = c.kaoServ.getKao();
	var sleepHpCount;
	var hungerHpCount;
	var playHpCount;
	var score = 0;
	var points = 0;
	var coreUpdate;

	//Button Controllers during gameplay
	c.actionSleep = function() {
		c.kaoServ.actionSleep(tmgch);
	};
	c.actionEat = function() {
		c.kaoServ.actionEat(tmgch);
	};
	c.actionPlay = function() {
		c.kaoServ.actionPlay(tmgch);
	};

	//Change menu screens
	c.mainMenu = function() {
		c.data.settings = c.kaoServ.setSetting('main');
	};
	c.settingsMenu = function() {
		c.data.settings = c.kaoServ.setSetting('settings');
	};
	//set difficulty
	c.setDif = function(level) {
		c.kaoServ.setLevel(level);
	};
```
2. This is where we'll start most of our game mechanics. Paste this next block below the previous one.
	1. First, we're calling on `spModal` to create a prompt for our player to enter a name for their Kaogotchi. Then we're setting the name and running the game function by calling `runGame();`.
	2. The `resumeGame()` function comes next, which picks up a Kaogotchi from the "Orphanage." Here you can see how you can leave the tab and return to an already active game by clicking on the `Resume` button from the HTML.
	3. The function `rungame()` shows that we are setting a few variables and requesting a server-side update once the game is created to go ahead and create a new record in the related table. We'll see more in Exercise 3 when we do the Server Script.

```javascript
	/* Exercise 2.2 */  
	
	c.startGame = function() {
		//To start a new game, open a modal and ask for a name
		spModal.open({
			title: 'Please, enter a name for your Kaogotchi:',
			input: true,
			value: c.name,
			buttons: [{
				label: '✘ ${Cancel}',
				cancel: true
			},
								{
									label: '✔ ${OK}',
									primary: true
								}
							 ]
		}).then(function(name) {
			//change to the game screen and start playing
			c.data.name = name;
			runGame();
		}, function() {
			//if there isn't a name given return to the menu screen
			return;
		});
	};

	c.checkResume = function() {
		//check to see if resuming a previous game is a possibility
		c.data.action = "checkResume";
		c.server.update().then(function() {
			//since we are calling the Server Script, we want to reset this so it doesn't have old information getting passed
			c.data.action = undefined;
		});
	};
	c.resumeGame = function() {
		//loads the prior game state and starts playing
		c.data.action = "actuallyResume";
		c.server.update().then(function() {
			c.data.action = undefined;
			if (c.data.resume) {
				tmgch = c.kaoServ.setTmgch(c.data.restoreTmgch);
				score = c.kaoServ.score(c.data.scoreBar);
				resumeGame();
			}
		});
	};
	function resumeGame() {
		//set the play screen
		c.data.settings = c.kaoServ.setSetting('play');
		//run the core function
		core();
		//keep running it in a loop
		coreUpdate = $interval(function() {
			core();
		}, 100);
	}
	
	function runGame() {
		//set the play screen
		c.data.settings = c.kaoServ.setSetting('play');
		//create a new Kao record
		c.data.action = "newKao";
		c.server.update().then(function() {
			c.data.action = undefined;
			//Start game
			core();
			//keep running the game in a loop
			coreUpdate = $interval(function() {
				core();
			}, 100);
		});
	}

```
3. This is where we'll get the bulk of the game's mechanics - Paste this one below the one we just pasted.
	1. Here's where we'll go over the `core()` function we called in our `runGame()` loop in the previous step. Here you'll see that we leverage the Angular Provider to fetch the current stats of the Kaogotchi, how we handle the score bar and adding points, and what happens when any of the stats hit zero.
	2. The next half shows how our stats are calculated and how we make sure they don't go over 100%, how the stats are set for `ngBind` to grab from, and the tick down of stats for the game. Then we update the server record to match the new values so if you close out of the page; your Kaogotchi is recoverable from the "Orphanage." (Aka. The table we've got these records in)
```javascript
	/* Exercise 2.3 */  

	//Main gameplay function
	function core() {
		//make sure we have the current stats held in the provider
		sleepHpCount = c.kaoServ.getSleep(tmgch.sleep);
		hungerHpCount = c.kaoServ.getHunger(tmgch.hunger);
		playHpCount = c.kaoServ.getPlay(tmgch.play);

		//Points can increase in fractions; only update the score when points are a whole number
		if ((points % 1) == 0) {
			score++;
			c.data.scoreBar = score;
		}
		//add points and round to 2 places
		points += c.kaoServ.addPoints();
		points = Math.round(points * 100) / 100;

		//get the Kaomoji face for the current stats
		c.server.get({
			action: 'getKaomoji',
			sleep: sleepHpCount,
			hunger: hungerHpCount,
			play: playHpCount
		}).then(function(r) {
			c.data.action = undefined;
			c.data.kaomoji = r.data.kaomoji;
		});

		//If you die, stop the loop and display the score
		if ((playHpCount <= 0) || (sleepHpCount <= 0) || (hungerHpCount <= 0)) {
			playHpCount = 0;
			sleepHpCount = 0;
			hungerHpCount = 0;
			$interval.cancel(coreUpdate);
			c.data.score = 'Your score: ' + score;
		}
		
		//do not allow any stat to go above 100%
		if ((tmgch.sleep / c.kaoServ.maxSleep * 100) > 100) {
			sleepHpCount = 100;
		}
		if ((tmgch.hunger / c.kaoServ.maxHunger * 100) > 100) {
			hungerHpCount = 100;
		}
		if ((tmgch.play / c.kaoServ.maxPlay * 100) > 100) {
			playHpCount = 100;
		}

		//Show stats on screen via ng-bind
		c.data.sleepHp = sleepHpCount;
		c.data.hungerHp = hungerHpCount;
		c.data.playHp = playHpCount;

		//Remove HP every loop
		tmgch.sleep -= 1;
		tmgch.hunger -= 3;
		tmgch.play -= 2;
		//update the server record to match the new values
		c.data.action = "updateKao";
		c.data.tmgch = tmgch;
		c.server.update().then(function() {
			c.data.action = undefined;
		});
	}
};
```

## Summary
And that's it! That's the Client Script; between it and the Angular Provider we'll set up in Exercise 4, that will be the bulk of the gameplay mechanics.

**Visual Verification:** At the end of the exercise, if you refresh your portal page, you shouldn't see anything other than the text showing the widget name. This is because we are still missing a large part of the game's code, and you are ok to keep moving forward in the lab.

# Exercise 3: Setting up the server side
## Goal
Now we will set up the Server Side script of the widget. This will be shorter, but it's essential as it's used to back up the game state. When you abandon a Kaogotchi mid-game, you'll get the option to resume when you return to the main page in the final version of the widget. You can even use this server-side functionality to create a leaderboard of high scores... check out Challenge #1 for more of my thoughts on this.

You can navigate to the page we'll be working from in one of two ways:
1. Head back to the Exercise 2 portal page you should have open (`kaogotchi_ex2_page`), then go ahead and click on the **Next Exercise** button at the bottom to navigate to the page you'll be working from. 
2. Go back to the landing page by following the link in the top right of the Exercise 2 portal page or going to `[instancename].lab.service-now.com/kao` and click on "I'm ready to start Exercise 3". 

 In the center of your screen, you should see a gray box with the text `Widget Name: Kaogotchi EX3 Widget`. This is the template for the widget you will be building; you can `CTRL+Right Click` on Mac and Windows inside the gray box, then click on **Widget in Editor** to open up the **Widget Editor** on a new tab. Saving your work as you go is an excellent habit to get into, so I'd suggest you hit `ALT+S` every so often.

## Instructions
1. Let's start with the first block of JavaScript for the Server Script section of our widget. Paste the code below at the top of the script box. We've got plenty of comments in the code, so check those out for more information.
```javascript
(function() {
	
	/* Exercise 3.1 */

    if (input) { //Check for "input" so that the Server script only runs when called
        //We are using GlideRecord in many places, so to simplify, we are just calling it here (instead of repeating it everywhere)
		var grXSKI = new GlideRecord('x_snc_kaogotchi_info'); 

        //input.action is what c.data.action from the Client Script looks like in the Server Script
        if (input.action == "checkResume") {
            //check to see if there is a game to resume and return the Name and Score
            grXSKI.addEncodedQuery("sys_created_by=" + gs.getUserName() + "^hunger>=1^play>=1^sleep>=1^sys_created_by!=guest^ORsys_created_by=NULL");
            grXSKI.orderByDesc('sys_created_on');
            grXSKI.setLimit(1);
            grXSKI.query();
            if (grXSKI.next()) {
                data.resume = true;
                data.name = grXSKI.getValue('name');
                data.scoreBar = grXSKI.getValue('score');
            }
        }

        if (input.action == "actuallyResume") {
            //load the saved state of an in-progress game
            grXSKI.addEncodedQuery("sys_created_by=" + gs.getUserName() + "^hunger>=1^play>=1^sleep>=1^sys_created_by!=guest^ORsys_created_by=NULL");
            grXSKI.orderByDesc('sys_created_on');
            grXSKI.setLimit(1);
            grXSKI.query();
            if (grXSKI.next()) {
                data.sys_id = grXSKI.getUniqueValue();
                data.name = grXSKI.getValue('name');
                data.restoreTmgch = {
                    hunger: grXSKI.getValue('hunger'),
                    play: grXSKI.getValue('play'),
                    sleep: grXSKI.getValue('sleep')
                };
                data.scoreBar = grXSKI.getValue('score');
                data.resume = true;
            }
        }
```
2. Next block will be us creating the record when we get `input.action == "newKao"` or updating the record when we receive `input.action == "updateKao"` from the Client side. This saves our data, so we can return to it later on by doing `input.action == 'getKaomoji'`. 
```javascript

			/* Exercise 3.2 */

        if (input.action == "newKao") {
            //start a new game
            grXSKI.initialize();
            grXSKI.setValue('name', input.name);
            grXSKI.insert();
            data.sys_id = grXSKI.getValue('sys_id');
        }

        if (input.action == "updateKao") {
            //update the record for this game
            if (grXSKI.get(input.sys_id)) {
                grXSKI.setValue('hunger', input.tmgch.hunger);
                grXSKI.setValue('play', input.tmgch.play);
                grXSKI.setValue('sleep', input.tmgch.sleep);
                grXSKI.setValue('score', input.scoreBar);
                grXSKI.update();
            }
        }

        if (input.action == 'getKaomoji') {
            //build the kaomoji to show on the screen
            data.kaomoji = x_snc_kaogotchi.Kaomoji(input.sleep, input.hunger, input.play);
        }
    }
})();
```

## Summary
Wasn't building that Server Side script quick and painless? Let's move on!

**Visual Verification:** At the end of the exercise, if you refresh your portal page, you shouldn't see anything other than the text showing the widget name. This is because we are still missing a large part of the game's code, and you are ok to keep moving forward in the lab.

# Exercise 4: Setting up the Angular provider
## Goal
Now we get to the cool part - the Angular Provider itself! We will build it, review its uses, then try out the whole thing in the next exercise. 

You can navigate to the page we'll be working from in one of two ways:
1. Head back to the Exercise 3 portal page you should have open (`kaogotchi_ex3_page`), then go ahead and click on the **Next Exercise** button at the bottom to navigate to the page you'll be working from. 
2. Go back to the landing page by following the link in the top right of the Exercise 3 portal page or going to `[instancename].lab.service-now.com/kao` and click on "I'm ready to start Exercise 4". 

 In the center of your screen, you should see a gray box with the text `Widget Name: Kaogotchi EX4 Widget`. This is the template for the widget you will be building; you can `CTRL+Right Click` on Mac and Windows inside the gray box, then click on **Widget in Editor** to open up the **Widget Editor** on a new tab. Saving your work as you go is an excellent habit to get into, so I'd suggest you hit `ALT+S` every so often.

## Instructions
1. Once in the **Widget Editor**, click on the hamburger menu (the three horizontal lines) in the top right corner and click **Open in Platform**. 
2. Scroll down to the bottom of the page and click on the **Angular Providers** tab. When creating your provider in the future, you'll want to click on **New**, but for now, click **Edit**.
3. Find the `KaogotchiService` provider from the list and move it to the right. Then click **Save**. 
4. Now that we're back on the widget page, you'll see the Angular Provider in the list and can access it from there, or you can go to the Widget Editor, refresh the page, and a box will appear in the top menu that lets you select and edit the provider from there. 
![See pictured: The Dependency Dropdown.](https://github.com/ServiceNowEvents/k23-CCL1256-K23/blob/main/kaogotchi_dependency_dropdown.jpg)
5. Paste the code below into the code box of the Angular Provider. Since there's no perfect place to break up the code, I'll have in-code explanations of each code segment.
```javascript
/**
* This Angular Provider is a shared space that we can store logic and values
* The logic is here so that we only need to write it once (instead of in each widget) and can centrally maintain it.
* The values are here so that we can store a value and share it to multiple widgets (removing the need to look up or 
* calculate the same thing repeatedly)
*/
 
 function kaogotchiServiceExample() {
    //first we declare all the variables that we will need for the functions below (with default values) 
    var setting = 'main';
    var maxTotal = 1000;
    var maxSleep = maxTotal;
    var maxHunger = maxTotal;
    var maxPlay = maxTotal;
    var day = maxTotal * 0.05;
    var tamagotchi = {
        sleep: maxSleep,
        hunger: maxHunger,
        play: maxPlay
    };
    var score = 0;
    var kaoName;

	//Below we are going to put all the reusable logic that we are sharing.
	//To make it available you need to add the name of this Provider to the "api.controller" in your Client Controller
	//(and the related list). Then you can call it similarly to how you would use a Script include.
    return {
		//When this function is called we take use the stored value for "day" to return a value
		//We are using the value of "day" as a stand in for the difficulty level
		//Easy = more points, Hard = less points
        addPoints: function() {
            return day / 100;
        },

        //Returns the current gameplay object, see also: setTmgch
		//This is useful to get the widgets set up with the same data (since it gets stored here)
        getKao: function() {
            return tamagotchi;
        },

		//The next three functions are run when an "action" is taken (i.e. you click a gameplay button)
        //When sleeping, eating, and playing we need to update the gameplay object, and then return the new state of it.
        //We also use this as a chance to make sure that the values cannot go over the maximum
		//(no matter how much you click you can't go over 100%)
        actionSleep: function(tmgch) {
            tmgch.sleep += day;
            if (tmgch.sleep >= maxSleep) {
                tmgch.sleep = maxSleep;
            }
            return tmgch;
        },
        actionEat: function(tmgch) {
            tmgch.hunger += day;
            if (tmgch.hunger >= maxHunger) {
                tmgch.hunger = maxHunger;
            }
            return tmgch;
        },
        actionPlay: function(tmgch) {
            tmgch.play += day;
            if (tmgch.play >= maxPlay) {
                tmgch.play = maxPlay;
            }
            return tmgch;
        },

		//If we are resuming a game in progress we retrieve the game state from the server and then store it here.
		//That way it is available to the client, and by saving it here can be shared across widgets. See also: getKao
        setTmgch: function(restoreTmgch) {
            tamagotchi = restoreTmgch;
            return tamagotchi;
        },

		//We are using the value of "day" as a stand in for the difficulty level
		//Easy = more points, Hard = less points
		//This function takes in the value for difficulty and then calls the "setDay" function to save it.
        setLevel: function(level) {
            if (level == "Hard") {
                this.setDay(maxTotal * 0.01); //calls the `setDay` function that is contained in this provider
            } else if (level == "Normal") {   //(included as an example of how you can call other functions in the provider from within the provider)
                this.setDay(maxTotal * 0.05);
            } else if (level == "Easy") {
                this.setDay(maxTotal * 0.1);
            }
        },

		//This function saves the value of "day". In this example it is only being called by the functions above.
		//Because the function is being called by another function we need to call it this way: this.setDay(value)
        setDay: function(dayVal) {
            day = dayVal;
        },

		//When showing the user their Kao's stats we use percentages to make it easy.
		//These functions take the raw score values and makes them into a percentage, rounded to two decimal places
		getSleep: function(sleep) {
            return (sleep / maxSleep * 100).toFixed(2);
        },
        getHunger: function(hunger) {
            return (hunger / maxHunger * 100).toFixed(2);
        },
        getPlay: function(play) {
            return (play / maxPlay * 100).toFixed(2);
        },

        //'setting' is the value that we are using to know what the game state is (i.e. are we on a menu or playing?)
        //This function lets all the widgets know what the game state is
        getSetting: function() {
            return setting;
        },

        //When we change the game state we need to store it here so that all the widgets can know what the state is. 
		//We also return it (this makes sure that the widget that tried to set it knows that it got set here)
        setSetting: function(state) {
            setting = state;
            return setting;
        },

        //This is a 2-in-1 function.
		//(we built some of these using different conventions to show different ways that you can code some of these things)
        //If you pass it a score (i.e. when you are resuming a game) it will store and return it (so you know it has been successfully stored), otherwise it will just return the current score
        score: function(resumeScore) {
            if (resumeScore) {
                score = resumeScore;
            }
            return score;
        },

        //This is built to do the same as the Score function above, but for the Name
        //When resuming a game the name will get retrieved from the server in one widget, but displayed by another
        name: function(resumeName) {
            if (resumeName) {
                kaoName = resumeName;
            }
            return kaoName;
        }
    };
}
```

## Summary
And that's it for the code of the widget! You have attached the Angular Provider to your widget and have added the code. Pat yourself on the back; almost time to see your finished work. As a quick review from the beginning, remember that for widgets to be able to "listen" to an Angular Provider, you must attach it to the widget record as we did and then call it from the Client Script's function as a dependency. 

**Visual Verification:** At the end of the exercise, if you refresh your portal page, you should see a single widget with the working Kaogotchi game inside. You should be able to play with it, seeing how it performs and how the different parts of the code all work together.

Something else that's important to note is the difference between an Angular Factory and an Angular Service. You may have noticed just now that the record we updated was a Service, but what exactly is the difference you may ask?

## Angular Factory vs Angular Service
When creating a new Angular Provider, there's a dropdown on the page that has you set whether it's a Factory or a Service. In ServiceNow's implementation of AngularJS, both Angular provider services and Angular provider factories are used to create objects or services that can be used throughout your application.  

The main difference between the two lies in how they create and return objects or services. 

An Angular provider service is a type of provider that returns an instance of a service. It is created using the `service()` method and is instantiated using the `new` keyword. Once instantiated, the service is cached and reused throughout the application.

An Angular provider factory, on the other hand, is a type of provider that returns a function that can be used to create and return objects or services. It is created using the `factory()` method and is responsible for creating and returning a new instance of the service every time it is called.

So, the main difference between the two is that an Angular provider service creates a singleton instance of the service, which is shared throughout the application. Whereas, an Angular provider factory creates a new instance of the service every time it is called.

When deciding which type of provider to use in your ServiceNow application, consider whether you need a singleton instance of the service that is shared throughout the application or if you need a new instance of the service every time it is called.

# Exercise 5: Split the widget up into multiple parts
## Goal
The original goal of this lab was for you to go through and split up the widget at this point, so you could see how they all can piece together. But the thought of making you copy and paste 3 Widgets worth of HTML, Client Script, and Server Script made me think that anyone would stop the lab at this point... so let's not do that. 

Instead, we will look at what the widgets look like separate from each other and point out a few things here and there. You can navigate to the page we'll be working from in one of two ways:
1. Head back to the Exercise 4 portal page you should have open (`kaogotchi_ex4_page`), then go ahead and click on the **Next Exercise** button at the bottom to navigate to the page you'll be working from. 
2. Go back to the landing page by following the link in the top right of the Exercise 4 portal page or going to `[instancename].lab.service-now.com/kao` and click on "I'm ready to start Exercise 5". 

The gray dotted lines show the borders of the three widgets you will be looking at; you can `CTRL+Right Click` on Mac and Windows inside each of them, then click on **Widget in Editor** to open up the **Widget Editor** on a new tab. 

## Instructions
1. Starting from the top down, go into the Kaogotchi Score widget by `CTRL+Right`, Clicking the top gray dotted box, and let's start to look at the code. 
	1. The HTML should be familiar to you; it's the score part of the original widget. Now you should be familiar with and understand the different Angular Directives used in the HTML. In line 2 you can see us calling the Angular Provider directly through `ng-if="c.kaoServ.getSetting() == 'play'"` to see whether we should display the inner div or not. `ngBind` is used in the rest of the HTML to show the values we are getting from the Client Script.
	2. The Client script looks way slimmed down, doesn't it? We're calling the service up at the top in line 4 and running the `scoreUpdate` function from here. We run a bit of the core function from here, mostly related to the points and losing the game. 
	3. And there's no Server Script! Where did it go?! We'll find it soon, don't worry.
2. Return to the exercise portal page and `CTRL+Right Click` on the second box to open it in the **Widget Editor**. 
	1. This widget is even more straightforward in comparison to the previous one as far as the HTML is concerned. It's mainly used to display the Kaomoji itself and the name of the Kaomoji.
	2. We have a very similar Client Script to the previous one. The `core()` function now only houses a reference to what triggers a game over, as well as grabbing information from the server for the current stats that will affect how the Kaomoji's expressions are built.
	3. Oh hey, it's a Server Script! This tiny script builds the Kaomoji to show it on the screen.
3. Return to the exercise portal one last time and `CTRL+Right Click` on the third box with the welcome message and buttons.
	1. Here's the rest of the HTML we have yet to see, where we build the buttons and the settings screen. 
	2. The Client Script here calls the Angular Provider at the top and then mainly sets up the actions that happen when you click the buttons. Here is where the Resume functionality lives, as you can find the `c.checkResume` and `c.resumeGame` functions. 
	3. Look at all this server script! This is where we do many of our server calls that are then shared through the Client Script to the Angular Provider. An example is the `actuallyResume` action in line 21, where we load the saved state of an in-progress game, and then send it to the Client Script using the `data` object. This is then received in the Client Script in lines 77 and 78 which then communicates the data to the Angular Provider to share with the other widgets.

## Summary
So that's the game! Feel free to play the game a little and see how fast the widgets talk to each other. Try to see how you can break the widgets, try to see how you high of a score you can get, and I hope you had fun learning how Angular Providers work! 

# Challenge 1: Set up a leaderboard
## Goal
Something that we thought would be awesome is to set up a leaderboard for whoever plays the game. The way that I think this could work is you would have the player play the game as normal, then when they get a game over they could enter in their initials and a total ranking of Kaomoji records from the `x_snc_kaogotchi_info` table would show in order with the highest score up top. This might be a fun activity to put in your instance's 404 page for example.

# Challenge 2: Change the numbers to progress bars
## Goal
What if instead of numbers, we showed progress bars as part of the score? Try to take on this challenge yourself and if you get stuck, check out the `Kaogotchi Control Bars` widget to see how Eric did this himself.

# The Minstrel's Ballad: The Unending Coil of Challenges (Extreme)
## Goal
This one's a toughie. So tough that I have no idea how to do it and if you think you can do it you should message me somehow (sign up on sndevs.com and message me @MGOPW) and show me how. 

Another super cool thing about Angular Providers is that they keep the information you gave them across multiple "pages" in your service portal. As long as you don't hard refresh your page, technically you should be able to have your Kaogotchi follow you across anything you're doing in a Service Portal... So what if somehow the widget was a small mini version of itself and it followed you across pages! It could be a tiny little window at the bottom right corner of your portal, that just hangs out with you while you re-open your incidents and delete your RITMs. Extra bonus points if you make it a drawer that lets you minimize and pause the game to get it out of the way and come back to it later.
