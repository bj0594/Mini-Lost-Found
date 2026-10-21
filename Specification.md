# Specification – Mini-lost-and-found

## 1 The assignment in my own words
A lost and found for managers that keeps track of what items have been lost at a festival, then returned.
The user can register a find, mark a pickup and return items.
Delivered as a GitHub repo with tests and a README.

## 2 Requirements

Main flow: A user mimics an attendant at the festival that registers a found item, sees the item in a list, marks it as picked up (by the employees), then marks it as returned when the owner surfaces.

Core (MUST):

- Add an item                  ✔ AddItem_ValidItem_AddsItemToList, PostItems_ValidItem_ReturnsCreatedItem
- List all items               ✔ AddItem_ValidItem_AddsItemToList (via GetItems), GetItems_AfterItemAdded_ReturnsOkWithList
- Mark an item as picked up    ✔ PickUpItem_ExistingItem_BecomesPickedUp, PatchItemsPickUp_ExistingItem_ReturnsNoContent
- Mark an item returned        ✔ ReturnItem_ExistingPickedUpItem, PatchItemsReturn_PickedUpItem_ReturnsNoContent
- REST API                     ✔ ItemsApiTests (every line in api.md)

Rules:

- Can't return an item before it has been picked up by an employee
- Can't claim an item as returned if it's already been returned

Later / bonus (NOT NOW):

- Delete an item

## 3 Flow
## 4 Interface
```
ItemService
- IReadOnlyList<Item> GetItems()            -> all books
- Item AddItem(string name)                 -> new item, IsPickedUp = false
- void PickUpItem(Guid id)                  -> item is picked up (IsPickedUp = true)
                                               already picked up: InvalidOperationException
- void ReturnItem(Guid id)                  -> item is returned
                                               not picked up: InvalidOperationException
- void DeleteItem(Guid id)                  -> item is removed

Item: Id (Guid), Name (string), IsPickedUp (bool), IsReturned (bool)
Example: 2b4c..., "glasses", true, false
```

Dependencies:

## 5 Test list

- [ ] A new item can be added
- [ ] An item can be marked as picked up
- [ ] An item can be marked as returned
- [ ] An item cannot be returned if not picked up
- [ ] An item cannot be claimed if it's already been returned
- [ ] An item can be deleted

## 6 Questions, assumptions and decisions
- Decision: the lost-and-found is at a festival (the items would be plenty and diverse)
- Decision: the domain will be placed in it's own project
- Decision: a claim for an already returned item will throw a 409 error
- Decision: Docker compose will include a health check

- Limitation: the application has no users, roles or login

- New project: yes
- Web API: yes (API)
- Real database: yes, PostgreSQL (DB)
- Docker: yes, Dockerfile and Docker Compose (DOCKER)
- Build and test on GitHub: not required (CI)
