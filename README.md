# React Focus Trap Hook for Refs
Lightweight and accessible focus trap react hook. Great for modals and dialog boxes.

## Install
```
yarn add @pofo/focus-trap
npm i @pofo/focus-trap
```

## 🚀 Features 🚀
* [x] Only enables tab movements within the focused target
* [x] Customize inital focus on the target
* [x] Resets initial focus after focused target goes away

## Basic Usage
```typescript
import { useRef, useState } from 'react';
import useFocusTrap from '@pofo/focus-trap';

const MyComponent = () => {
    
    const ref = useRef( null );
    const [ isActive, setIsActive ] = useState( false );

    const styles = {
        visibility: isActive ?? 'visible' : 'hidden';
    }

    useFocusTrap( ref, isActive );

    return (
        <>
            <button onClick={ setIsActive( state => !state ) }>Toggle the div below!</button>
            <div style={styles} ref={ref}>
                Hi!
                <button>Try clicking Tab!</button>
                <button>har har har</button>
            </div>
            <button>You can only tab me when the div above is not active!</button>
        </>
    )
}
```

# !IMPORTANT!
*useFocusTrap only works if there is AT LEAST one focuable element within the target. That one focusable element should probably be a close button to close the target*.

### Customizing Options
By default, nothing on the target is focused when activated. This can be customized through the third optional options parameter.

```typescript
    useFocusTrap( ref, isActive, {
        initialFocus: 'first', // focus the first available focusable element on the target
    } );
}
```

## API
```typescript
useFocusTrap<T extends HTMLElement>(
    // requried
    ref: RefObject<T>,
    // required
    isActive: boolean,
    // optional
    options: {
        initialFocus?: 'first' | 'none' | number,
        tabbableElems: 'string',
    },
)
```

| Parameter | Default | Type | Description |
| ----------- | ----------- | ----------- | ----------- |
| `ref` | `REQUIRED` | `RefObject<T>` | ref of the target | 
`isActive` | `REQUIRED` | `boolean` | isActive state representing whether the target is on the screen |
`options` | `{ initialFocus: 'none', tabbableElems: '' }` | `{ initialFocus: 'first' \| 'none' \| number, tabbableElems: string }` | Optional options object.<br/> **initialFocus**: When set to `'none'`, no element if focused on target mount. When set to `'first'`, the first tabbable element is given focus on the target. When set to a `number`, the corresponding order on the HTML tree structure within the target is given focus. So if you put 0, the first tabbable element will be given focus. 1, the next -> and so on.<br/> **tabbableElems**: Specify *additional* tabbable elements you want to add to the query selector. For example, you can add `', object'` to add the object element to the tabbable elements array. Always preface the string with `, `. By default, the following elements are tabbable:
```typescript
'a[href], button:not([disabled]), input:not([disabled]), select:not([disabled]), textarea:not([disabled]), area[href], form, audio[controls], video[controls], [tabindex="0"]'
```

## Extra
Plays well with [useClickOutsideRef](https://github.com/pofoo/click-outside)

## LICENSE
MIT