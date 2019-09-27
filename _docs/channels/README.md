# Channel

- [Introduction](#introduction)
- [Child Components](#child-components)
  - [ChannelSidebar](#channelsidebar)
  - [ChannelFilter](#channelfilter)
  - [ChannelOrder](#channelorder)
  - [ChannelOrderList](#channelorderlist)
- [Slots](#slots)
- [Props](#props)
- [Data Members](#data-members)
- [Mixins](#mixins)
  - [export](#export)
  - [import](#import)
  - [validator](#validator)
- [Methods]
- [Useage Example](#useage-example)


https://vue-styleguidist.github.io/docs/Documenting.html#code-comments

## Introduction

Users interact with orders using [Channels](). Channels group a client's cases
together and allows accessioners to accession a client in order. Each channel
has it's own folder in the `views/channels` folder.

The `channel.vue` component wraps four [display components](#display-components),
[data interactions](), and customized [mapping functions]().



## Child Components


### ChannelSidebar

Read more about the [ChannelSidebar](#)

| Event       | Local Handler   | description                                 |
|-------------|-----------------|---------------------------------------------|
| back        | onCancelOrder   | The back (arrow) button was pressed, so we show the ChannelOrderList and close any open ChanelOrder |
| filter      | onToggleFilter  | The filter (magnifier) button was pressed, and we toggle the view state. |
| refresh     | onRefreshOrders | The refresh button was pressed, so we reload the list based on teh current state of the `filter`. |


### ChannelFilter

Read more about hte [ChannelFilter Here](#)

We pass the channel filter a single prop using the `v-model` shorthand.

| Prop      | Type     | Description                                           |
|-----------|----------|-------------------------------------------------------|
| filter    | Object   | This is object we use to filter the ChannelOrderList  |

We watch that event for changes, and refresh the list based on teh changed
values.


### ChannelOrder

Read [ChannelOrder](#) to learn about this component.

We pass [ChannelOrder](#) these properties

| Prop           | Local variable | Type   | Description                           |
|----------------|----------------|--------|---------------------------------------|
| unmatchedOrder | unmatchedOrder | Object | The original order as downloaded from the API |
| v-model        | matchedOrder   | Object | The **matched* order after it passed through the matchers |
| lookups        | mcLookups      | Object | Lookups from the metaclinic API.   |

We have two events to handle

| Event        | Local Handler   | Description                                 |
|--------------|-----------------|---------------------------------------------|
| convert      | onConvertOrder  | The user has clicked on the Save to LIS button |
| cancel       | onCancelOrder   | We don't take any action. Return back to the original channel route (back) |


### ChannelOrderList

Read [ChannelOrderList](#) to learn more detail abou this component.

We pass [ChanelOrderList](#) a single property.

| Prop          | Type     | Description                                           |
|---------------|----------|-------------------------------------------------------|
| channelOrders | Array    | An Array of (filtered) orders available on the channel |

We also have one event to handle

| Event       | Local Handler   | description                                 |
|-------------|-----------------|---------------------------------------------|
| open        | onOpenOrder     | When the user selects an order, we hide the `ChannelOrderList, and show the `ChannelOrder` |



## Slots

None



## Props

** only router :orderID



## Data

| Data            | Type       | Description                                   |
|-----------------|------------|-----------------------------------------------|
| showFilters     | Boolean    | state for ChannelOrderFilter visibility       |
| showOrder       | Boolean    | state for ChannelOrder visiblity              |
| filter          | Object     | filter object used to filter orders for the channel |
| channelOrders   | Array      | the (filtered) list of orders available on the channel |
| lookups         | Object     | lookup data for matching the metaclinic LIS   |
| matchedOrder    | Object     | an order after it has been matched to LIS values |
| unmatchedOrder  | Object     | the original [Order](#) object as returned from the Apollo API |



## Mixins


From `/views/core`
* import.js
* export.js
* validator.js



## Methods

** list with links to each one.
| method          | Use                                                        |
|-----------------|------------------------------------------------------------|
| refreshOrders   | Refresh the orders view when the ChannelSidebar refresh button is clicked. |
| toggleFilters   | Toggle the ChannelFilter's visibility.                     |
| closeOrder      | Closes the ChannelOrder view if it's open                  |


##  Useage Example

