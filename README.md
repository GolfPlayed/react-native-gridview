# React Native GridView Component

Here is how it looks:

![alt text](https://s24.postimg.org/ir4zxi25x/Simulator_Screen_Shot_Feb_7_2017_15_09_37.png "iPhone 5s")

## Instalation

Via NPM

`npm install react-native-easy-gridview --save`

## Why do you need this?

If you try to make a grid view in React Native the way it is described in the docs you will encounter several problems.
You need to specify exact width in pixels of your items in the grid. To do that you need to measure the available width for the whole grid, and divide that number by the number of columns you wish to have. There is no way to use percentages (they are not yet supported in RN) or flexbox to define the width of items, you have to do these calculations manually.

When you divide the available width with the number of columns, you will get a number that is most likely not a whole number, and antialiasing will kick in when rendering borders of your items. You might not care about this problem, but if you have 1px borders of your items they will not look nice and crisp.

This is why I created this component. It will fix antialiasing issues and do all these measuring and calculations for you. It uses ListView component underneath (so performance is good), and therefore accepts all the props that the original RN ListView component receives, plus one extra prop to set the number of columns (`numberOfItemsPerRow`).

## Example

To run the example just do `npm install` in the `/Example` dir and then run `react-native run-ios`, or open it in Xcode and run it from there.

Or you can copy this piece of code and try it out yourself:

```javascript
import React, { Component } from 'react';
import {AppRegistry, View, StyleSheet, ListView, Text} from 'react-native';
import GridView from 'react-native-easy-gridview';

const styles = StyleSheet.create({
    listContainer: {flex: 1, backgroundColor: 'powderblue'},
    item: {backgroundColor: 'navajowhite', margin: 3, paddingVertical: 7, borderWidth: 4, borderColor: 'orange', alignItems: 'center', justifyContent: 'center'}
});

const Example = React.createClass({
    getInitialState: function() {
        var ds = new ListView.DataSource({rowHasChanged: (r1, r2) => r1 !== r2});
        var data = Array.apply(null, {length: 40}).map(Number.call, Number);

        return {
            dataSource: ds.cloneWithRows(data)
        };
    },

    render: function() {
        return (
            <View style={styles.listContainer}>
                <GridView
                    dataSource={this.state.dataSource}
                    renderRow={this._renderRow}
                    numberOfItemsPerRow={5}
                    removeClippedSubviews={false}
                    initialListSize={1}
                    pageSize={5}
                />
            </View>
        );
    },

    _renderRow: function(rowData) {
        return (
            <View style={styles.item}>
                <Text>{rowData}</Text>
            </View>
        );
    }
});
```

## Props

`numberOfItemsPerRow` - Like the name implies, it's the number of items per row (number of columns).

[ListView props...]

**Tip:** set `pageSize` to be the same as `numberOfItemsPerRow` to avoid rendering issues with ListView.
