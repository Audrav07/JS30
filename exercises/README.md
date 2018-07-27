# Local Storage and Event Delegation

![photo](screen.png)

## Objective:
* Local storage: On a tapas menu - if you want the item you can check and uncheck the boxes. If you add a new item to the menu and click refresh it is still there. Persisting our state with local storage.

* Event delegation: When adding another item and click on it you can check it right away because if you add an event listener on something that doesn't exist you'll know that in the future it won't be clicked. Use event delegation to fix that.

## Steps:

### Take and load data with local storage

``  const addItems = document.querySelector('.add-items');
  const itemsList = document.querySelector('.plates');
  const items = [];``

* The localStorage property allows you to access a local Storage object

``function addItem(e) {
	e.preventDefault();
  const item = {
    text: text,  // or in ES6 syntax: `text,`
    done: false
  };
  items.push(item);
  populateList(items, itemsList);
  // localStorage.setItem ('items', items);
  localStorage.setItem ('items', JSON.stringify(items));
  this.reset();
}``

* Declare a function that will be responsible for generating the necessary HTML to display each item in the items array as a list item with a clickable checkbox.

``const populateList = (items, itemsList) => {
  // If the parameter `items` isn't an array, go no further
  if (!(items instanceof Array)) return false
  // Update the 'itemsList' HTML to contain each item stored in the `items` array
  itemsList.innerHTML = items.map((item, i) => {
    return `
      <li>
        <input type="checkbox" data-index=${i} id="item${i}" ${item.done ? 'checked' : ''} />
        <label for="item${i}">${item.text}</label>
      </li>
    `
  }).join('')
}``

* Go back to the event handler function body and call the newly defined function after we've pushed an item object into the items array:

``/* Right after 'items.push(item)' */
populateList(items, itemsList)``

* At the very top of your JavaScript code, change const items so that it is either an array generated from parsing through the items currently in localStorage, or if there is nothing in localStorage, an empty array.
    ``const addItems = document.querySelector('.add-items'),
      itemsList = document.querySelector('.plates'),
      items = JSON.parse(localStorage.getItem('dishes')) || []; // This is the line we're changing``

* We need to call the populateList method as soon as the document loads so that we can generate the menu items if there is something in the localStorage:

``document.onload = populateList(items, itemsList)``

* Define this event handler function so that it accepts an event as a parameter and, if the target of the event contains the text 'input' anywhere, we'll toggle the done value of correct object in the items array and update the localStorage to reflect that change:

``function toggleDone(e) {
  if(!e.target.matches('input')) return;
  const el = e.target;
  const index = el.dataset.index;
  items[index].done = !items[index].done;
  localStorage.setItem ('items', JSON.stringify(items))
  populateList(items, itemsList);
}``
