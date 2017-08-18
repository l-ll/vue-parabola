# vue-parabola

a vue-parabola base & inspired by [react-native-smart-parabola](https://github.com/react-native-component/react-native-smart-parabola)

# Usage
```javascript
    const el = e.target.getBoundingClientRect()
    const shopCart = document.querySelector('.icon-shop')
    const shopCartBox = shopCart.getBoundingClientRect()
    const start = {
      x: el.left,
      y: el.top
    }
    const end = {
      x: shopCartBox.width / 2 + shopCartBox.left - 10,
      y: shopCartBox.height / 2 + shopCartBox.top - 10
    }
    this.$parabola({
      start: start,
      end: end,
      isTrigger: true,
      onParabolaFinish: () => {
        console.log('onParabolaFinish')
      }
    })
```

# Props

Prop              | Type   | Optional | Default   | Description
----------------  | ------ | -------- | --------- | -----------
start             | object | No       |           | determine the start coordinate (pageX, pageY)
end               | object | No       |           | determine the end coordinate (pageX, pageY)
isTrigger         | bool   | No       |           | If the value is true, a new parabola view will be rendered
top               | number | Yes      | 0         | determine the extra offset of axis y
duration          | number | Yes      | 500       | determine the parabola animation duration
rate              | number | Yes      | 1         | determine the value which can change the parabola trajectory
onParabolaFinish  | function | Yes    |           | onParabolaFinish callback
