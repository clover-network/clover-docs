# Setup dapp project

## 🌽 Create a project using create-react-app

Let's start with creating a frontend project using create-react-app. 

```
npx create-react-app counter-dapp
```

{% hint style="info" %}
 This command takes a little while to complete, be pertinent. You need to confirm the installation of create-react-app if it's your first time using this command.
{% endhint %}

Once the command complete, start the web app:

```bash
cd counter-dapp
yarn start
```

{% hint style="info" %}
A browser window will be opened and who the web app. You can visit [http://localhost:3000/](http://localhost:3000/) either. 
{% endhint %}

## 🛠 Add counter state and buttons

We'll add a simple text to show the current value of the counter state.

"Inc" and "Dec" buttons to update the counter state. 

Edit `src/App.js` and set the `App()` function looks like below:

```jsx
function App() {
  return (
    <div className="App">
      <header className="App-header">
        <h1>Counter Example</h1>
        <p>
            Current value: n/a
        </p>
        <button className="CounterButton">Inc Counter</button>
        <button className="CounterButton">Dec Counter</button>
      </header>
    </div>
  );
}
```

Edit `src/App.css` and add the css class `CounterButton`. You can make your customizations as you like.

```css
.CounterButton {
  background-color: #4CAF50;
  border: none;
  color: white;
  margin-top: 15px;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  width: 200px;
}
```

The webpage will be reloaded after you saved the files.

Now you can see we're having the basic page layout!

{% hint style="info" %}
The source code of this chapter could be found at the revision `fdb1b9e5f` in the [`counter-dapp`](https://github.com/clover-network/example-counter-dapp) source repo.
{% endhint %}

