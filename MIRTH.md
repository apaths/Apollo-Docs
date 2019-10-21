## MIRTH

We use mirth to handle inbound orders for the HMR. MIRTH takes care of formatting
the HL7 (or other formats)  messages from the customer's EMR system. MIRTH injects
the orders via the [Orders API]().

After orders are created they may be viewed using a specific customer [View]().

^^^^^^^^^^^^^^^^^^^^^^^^^

# Channels

Users interact with orders using [Channels](). Channels group a client's cases
together and allows accessioners to accession a client in order. Each channel
has it's own folder in the `views/channels` folder.

Every channel has a similar structure, and similar components. The main compnent
is the `channel.vue` single file component.


## `Channel.vue`

`Channel.vue` is a single-file component. It wraps four [display components](#display-components),
[data interactions](), and customized [mapping functions]().


### Display Components

Each `channel.vue` has four presentational components. These components are imported
from the `views/core` folder. All four views are common to ALL channels. This means
we display orders ina common manner for every inbound channel we create. Our users
enjoy a consistent view and our codebase is more maintainable.


#### Sidebar

The `sidebar` component provides a pallet for icon buttons. Use this area for
simple operations like toggling the [Filter](#filter) sidebar, refreshing data,
or providing a back button.


#### Filter

The `filter` component give users ability to limit cases displayed in
the [OrderList](#orderlist) component.


#### OrderList

The `OrderList` shows the results of any conditions set in the `filter`
component.


#### Order

Finally the `Order` component shows an order selected from the `OrderList`.


### Data interactivity

Each Channel

The goal of Channels is translating [Orders]() into Cases in the Metaclinic LIS.
Often channels will need customized translation operations. Channel components
are specifically designed to support customization while reusing as much common
conde as possible.

This data interactivity  happens in three steps.

#### Matching order values to LIS values

First, we match the inbound [Order]() values to LIS values. Often orders coming
from EMRs will contain typo errors or inconsistent spellings. In the `matcher.js`
file we provide attempt to match [Order]() values with lookup values in the LIS.

Initial value matching happens in `channel.vue`'s `openOrder()` method. We use
a default matching file -- `views/core/matcher.js` and a customizable `matchers.js`
in the channel's folder to created the matched values.


##### `views/core/matcher.js`








each client. Channels wrap four
single file components.

 that are common to all channels. These components handle
the *presentation* to the user.

Channels then use Vue events model to handle the

Often customer or
EMR will have unique customization needs when translating [`order`]() values to
LIS cases. Channel components are specifically structured to allow easy
customization where necessary while reusing as much common code as possible.


