## Create the mockup
This practice is based on the BS example. 

## Break the UI into components
A component should ideally only do one thing.
- Image
- Title
- Price
- Stock
- Buy
- T-shirt product
- Main application

## Build a static version in React
1. Define the components by writing a JavaScript functions. Specify the JSX code that represents the HTML-like structure to be rendered on the screen.
```
function Image () {
  return <img className="img-fluid" src="images/blue-t-shirt.jpg" alt="Blue T-Shirt" />
}
```

Identify the dynamic data
```
function Title (props) {
  return <h2>{props.title}</h2>
}
```

Destructure the props object and extract the price property
```
function Price ({ price }) {
  return <p><strong><em>$ {price}</em></strong></p>
}
```

```
function Stock ({ stock }) {
    return <p>{stock} left!</p>
}
```

```
function Buy ({ stock, quantity }) {
    return (
      <form>
        <div className="input-group mb-3">
          <input type="number" className="form-control" placeholder="0"  min="1" max={stock} />
          <button type="submit" className="btn btn-outline-secondary" >Buy</button>
        </div>
      </form>
    )
}
```

```
function Tshirt ({ tshirt }) {
 
  return (
    <div className="col col-12 col-md-6 col-lg-4 my-3">
      <Image title={tshirt.title} image={tshirt.image} />
      <Title title={tshirt.title} />
      <Price price={tshirt.price} />
      <Stock stock={tshirt.stock} />

      <div className="row">
        <div className="col-4">
          <Buy stock={tshirt.stock} quantity={tshirt.quantity} />
        </div>
      </div>
    </div>
  )
}
```

```
function App () {
  return ( 
    <div className="container">
      <div className="row">
        <div className="col">
          <h1>T-Shirts</h1>
        </div>
      </div>
      <div className="row">
        {tshirts.map(tshirt => <Tshirt key={tshirt.id} tshirt={tshirt} />)}
      </div>
    </div>
  )
}

```

2. Create the root object and render the main component
```
const root = ReactDOM.createRoot(document.querySelector('#root'))
root.render(<App />)
```

## Identify and define state variables

1. Identify variables to store and manage data that can change over time.
- stock
- quantity

2. Identify where the state variables should be placed. Often, you can put them into their closest common parent component.
- T-shirt product

```
const [stock, setStock] = React.useState(tshirt.stock)
const [quantity, setQuantity] = React.useState(tshirt.quantity)
```

Update the value of the props 
```
stock={stock} quantity={quantity}
```

## Inverse flow
### Buy

1. In the buy component, extract the 'onBuy' property from destructuring the the props object
```
function Buy ({ stock, onBuy }) { }
```
2. Call the 'onBuy' custom event (defined in the parent) when the click event occurs in the button.
```
onClick={onBuy}
```

3. In the parent component, define the custom event property and call the 'buyHandler' method
```
<Buy stock={stock} quantity={quantity} onBuy={buyHandler} />
```

4. Define the buyHandler, it should update the stock
```
function buyHandler() {
    setStock(stock - quantity)
  }
```

### Quantity

1. In the buy component, extract the 'quantity' property from destructuring  the props object
```
function Buy ({ stock, onBuy, quantity }) { }
```
2. In the input element define the value equals to quantity
```
<input type="number" value={quantity} ...
```

1. Call the 'onQtyChange' custom event (defined in the parent) when the input changes its value. Send the value as a parameter 
```
onChange={(e) => onQtyChange(e.target.value)}
```

2. In the parent component, define the custom event property and call the 'qtyChangeHandler' method
```
 <Buy stock={stock} quantity={quantity} onBuy={buyHandler} onQtyChange={qtyChangeHandler} />
```

3. Define the qtyChangeHandler, it should update the quantity.
```
  function qtyChangeHandler(value){
    setQuantity(value)
  }
```
4. Validate the stock vs quanity. The user can buy only when quanity is less or equals to stock.
```
function buyHandler() {
    if(stock >= quantity) {
      setStock(stock - quantity)
    }
  }
```
5. Add aditional functionality
```
setQuantity(1)
```
```
function Stock ({ stock }) {
  return stock ? <p>{stock} left!</p> : <p className="text-danger">Out of stock</p>
}
```


