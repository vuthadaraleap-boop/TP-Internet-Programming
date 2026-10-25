# Week 3 Answers

1. **Array index as `:key`.**
   With an index key, Vue identifies a card by its position in the list. When the list is filtered, a different event moves into the same position, so Vue reuses the old card and keeps its local state, such as an open details panel. The panel then shows up on the wrong event. With `:key="e.id"`, each card keeps its own identity and state, even when the list changes.

2. **Why `visible` is a computed.**
   A computed is cached and only recalculates when `search`, `category` or `events` change. A function called from the template runs again on every re-render, even when something unrelated changes. When you type in the search box, both give the same result, but the computed does no extra work on unrelated updates.

3. **Why EventCard emits `book`.**
   First, props are read-only, so changing `event.booked` in the card would break one-way data flow, and Vue would warn. Second, the parent owns the data and the rules, such as the seat limit and `myBookings`. The card only reports "the user wants to book event 7", so it stays reusable and the logic stays in one place.

4. **`const { booked } = reactive(event); booked++`.**
   Destructuring copies the plain number out of the reactive object. The variable `booked` is no longer connected to Vue's reactivity, so `booked++` only changes a local variable and nothing re-renders. To keep the connection, change `state.booked++` on the reactive object, or use `toRefs`.

5. **Where `myBookings` lives.**
   It lives in `App.vue`, because App owns the events and is the one that changes them in `onBook`. The header, which is also in App, needs the count. If it lived in EventCard or EventList, each card would have its own copy, and the header could not see it.