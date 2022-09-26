1. Explain what the simple `List` component does.
  The List component displays the list items in the array.The items are mapped through each item and displays the text.

2. What problems / warnings are there with code?

  a - the useState is being misplaced as in :
   ```const [setSelectedIndex, selectedIndex] = useState();```
   
   instead_ it has to be:
   
   ``` const [selectedIndex,setSelectedIndex] = useState();```
 
  b - The items list is null so we can't map through each item.
  
  ```WrappedListComponent.defaultProps = {
   items: null,
   };
  ```
  
  c - isSelected state holds the type of Boolean, but while passing the props it returns a number. 
  
  ```isSelected: PropTypes.bool,```
 
  d - Each list item must a unique key prop.

 ```Javascript
  `{items.map((item, index) => (
       <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex}
     />
  ))}`

  ```
3. Please fix, optimize, and/or modify the component as much as you think is necessary.

## Code

```javascript
import React, { useState, useEffect, memo } from 'react';
      import PropTypes from 'prop-types';

      // Single List Item
      const WrappedSingleListItem = ({
      index,
      isSelected,
     onClickHandler,
     text,
     }) => {
       return (
            <li
              style={{ backgroundColor: isSelected ? 'green' : 'red'}}
             onClick={() => onClickHandler(index)}
          >
          {text}
         </li>
        );
   };

  WrappedSingleListItem.propTypes = {
       index: PropTypes.number,
       isSelected: PropTypes.bool,
      onClickHandler: PropTypes.func.isRequired,
      text: PropTypes.string.isRequired,
 };

 WrappedSingleListItem.defaultProps = {
     isSelected: false
 };

 const SingleListItem = memo(WrappedSingleListItem);

 const WrappedListComponent = ({
     items,
 }) => {
      const [selectedIndex,setSelectedIndex] = useState();

      useEffect(() => {
           setSelectedIndex(null);
     }, [items]);

  const handleClick = index => {
     setSelectedIndex(index);
  };

  return (
       <ul style={{ textAlign: 'left' }}>
          {items.map((item, index) => (
             <SingleListItem
                 onClickHandler={() => handleClick(index)}
                 text={item.text}
                  key={index}
                 isSelected={selectedIndex === index}
            />
         ))}
      </ul>
 )};

WrappedListComponent.propTypes = {
     items: PropTypes.arrayOf(PropTypes.shape({
         text: PropTypes.string.isRequired,
    })),
};

 WrappedListComponent.defaultProps = {
      items: [{text: "ABC"}, {text: "XYZ"}],
 };

const List = memo(WrappedListComponent);

export default List;

```