---
layout: post
title:  "TodoMVC In Javascript Dom Scripting"
date:   2023-03-15 00:08:59 +0800
categories: javascript dom
---

I created this simple implementation based on dom programming for guiding a friend started with web development.

This is actually just a way of different implementations. It is written in pure js, and data driven rendering. Very fast forward way of coding.

code sctructure like this:

```js
// data (runtime and reactive)
const state = {}

// set data and trigger render()
function setState() {}

// rendering
function render() {
  renderA();
  addEventsToA();

  renderB();
  addEventsToB();
  
  // ...
  
  addEventsToGlobal();
}

// first time of render()
render();
```

So the final implementation is:

## index.html

```html
<!doctype html>
<html lang="en">
	<head>
		<meta charset="utf-8">
		<meta name="viewport" content="width=device-width, initial-scale=1">
		<title>Template • TodoMVC</title>
		<link rel="stylesheet" href="node_modules/todomvc-common/base.css">
		<link rel="stylesheet" href="node_modules/todomvc-app-css/index.css">
		<!-- CSS overrides - remove if you don't need it -->
		<link rel="stylesheet" href="css/app.css">
	</head>
	<body>
		<section class="todoapp">
			<header class="header">
				<h1>todos</h1>
				<input class="new-todo" placeholder="What needs to be done?" autofocus>
			</header>
			<!-- This section should be hidden by default and shown when there are todos -->
			<section class="main">
				<input id="toggle-all" class="toggle-all" type="checkbox">
				<label for="toggle-all">Mark all as complete</label>
				<ul class="todo-list">
					<!-- These are here just to show the structure of the list items -->
					<!-- List items should get the class `editing` when editing and `completed` when marked as completed -->
					<li class="completed">
						<div class="view">
							<input class="toggle" type="checkbox" checked>
							<label>Taste JavaScript</label>
							<button class="destroy"></button>
						</div>
						<input class="edit" value="Create a TodoMVC template">
					</li>
					<li>
						<div class="view">
							<input class="toggle" type="checkbox">
							<label>Buy a unicorn</label>
							<button class="destroy"></button>
						</div>
						<input class="edit" value="Rule the web">
					</li>
				</ul>
			</section>
			<!-- This footer should be hidden by default and shown when there are todos -->
			<footer class="footer">
				<!-- This should be `0 items left` by default -->
				<span class="todo-count"><strong>0</strong> item left</span>
				<!-- Remove this if you don't implement routing -->
				<ul class="filters">
					<li>
						<a class="selected" href="#/">All</a>
					</li>
					<li>
						<a href="#/active">Active</a>
					</li>
					<li>
						<a href="#/completed">Completed</a>
					</li>
				</ul>
				<!-- Hidden if no completed items are left ↓ -->
				<button class="clear-completed">Clear completed</button>
			</footer>
		</section>
		<footer class="info">
			<p>Double-click to edit a todo</p>
			<!-- Remove the below line ↓ -->
			<p>Template by <a href="http://sindresorhus.com">Sindre Sorhus</a></p>
			<!-- Change this out with your name and url ↓ -->
			<p>Created by <a href="http://todomvc.com">you</a></p>
			<p>Part of <a href="http://todomvc.com">TodoMVC</a></p>
		</footer>
		<!-- Scripts here. Don't remove ↓ -->
		<script src="node_modules/todomvc-common/base.js"></script>
		<script src="js/app.js"></script>
	</body>
</html>

```

## js/app.js

```js
(function (window) {
	"use strict";

	// Your starting point. Enjoy the ride!
	const state = {
		todos: JSON.parse(localStorage.getItem("todomvc.todos")) || [], // [{id, title, completed}]
		filters: [],
		currentFilter: location.hash || "#/",
		get isAllCompleted() {
			return !this.todos.find((todo) => !todo.completed);
		},
		get activeCount() {
			return this.todos.filter((todo) => !todo.completed).length;
		},
		get hasCompleted() {
			return !!this.todos.find((todo) => todo.completed);
		},
	};

	function setState(newState) {
		Object.assign(state, newState);
		afterStateChange(state, newState);
		renderPage();
	}

	const app = {
		newInput: document.querySelector(".new-todo"),
		toggleAllBtn: document.querySelector(".toggle-all"),
		todoList: document.querySelector(".todo-list"),
		todoItemTemplate: document.querySelector(".todo-list li").cloneNode(true),
		filterItems: document.querySelectorAll(".filters a"),
		todoCount: document.querySelector(".todo-count strong"),
		clearCompletedBtn: document.querySelector(".clear-completed"),
	};

	function afterStateChange(state, newState) {
		if (newState.hasOwnProperty("todos")) {
			localStorage.setItem("todomvc.todos", JSON.stringify(state.todos));
		}
	}

	function renderPage() {
		// render toggle all
		app.toggleAllBtn.checked = state.isAllCompleted;
		app.toggleAllBtn.addEventListener("change", (e) => {
			setState({
				todos: state.todos.map((todo) => {
					todo.completed = e.target.checked;
					return todo;
				}),
			});
		});

		// render new todo
		app.newInput.addEventListener("keypress", (e) => {
			if (e.key === "Enter" && e.target.value.trim()) {
				state.todos.push({
					id: Date.now(),
					title: e.target.value,
					completed: false,
				});
				setState({ todos: [...state.todos] });
				app.newInput.value = "";
			}
		});

		// render todo list
		app.todoList.innerHTML = "";
		state.todos
			.filter((todo) => {
				if (state.currentFilter === "#/") {
					return true;
				} else if (state.currentFilter === "#/active") {
					return !todo.completed;
				} else if (state.currentFilter === "#/completed") {
					return todo.completed;
				} else {
					return true;
				}
			})
			.forEach((todo, index) => {
				const li = app.todoItemTemplate.cloneNode(true);
				li.classList.toggle("completed", todo.completed);
				li.querySelector(".toggle").checked = todo.completed;

				li.querySelector("label").innerText = todo.title;
				li.querySelector("label").addEventListener("dblclick", (e) => {
					li.classList.toggle("editing", true);
					li.querySelector(".edit").value = todo.title;
					li.querySelector(".edit").focus();
				});

				li.querySelector(".edit").value = todo.title;
				li.querySelector(".edit").addEventListener("blur", (e) => {
					e.preventDefault();
					todo.title = e.target.value;
					setState({ todos: [...state.todos] });
				});
				li.querySelector(".edit").addEventListener("keypress", (e) => {
					if (e.key === "Enter") {
						li.classList.toggle("editing", false);
					}
				});

				li.querySelector(".toggle").addEventListener("change", (e) => {
					todo.completed = e.target.checked;
					setState({ todos: [...state.todos] });
				});

				li.querySelector(".destroy").addEventListener("click", () => {
					state.todos.splice(index, 1);
					setState({ todos: [...state.todos] });
				});

				app.todoList.appendChild(li);
			});

		// render counts
		app.todoCount.innerText = state.activeCount;

		// render filters
		app.filterItems.forEach((anchor) => {
			const url = new URL(anchor.href);
			anchor.classList.toggle("selected", url.hash === state.currentFilter);
		});

		// render clear completed
		app.clearCompletedBtn.hidden = !state.hasCompleted;
		app.clearCompletedBtn.addEventListener("click", (e) => {
			e.preventDefault();
			setState({ todos: state.todos.filter((todo) => !todo.completed) });
		});

		// global hash change
		window.addEventListener("hashchange", (e) => {
			const url = new URL(e.newURL);
			setState({ currentFilter: url.hash });
		});
	}

	renderPage();
})(window);
```
