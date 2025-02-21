# Multi-chain toggle frontend component

The following section is a guide to working with React components to switch chains and make calls to desired RPC URLs in TypeScript.

### The component[​](https://icarus131.github.io/devcookbook/docs/SwitchingChains#the-component) <a href="#the-component" id="the-component"></a>

The [TabbedButtons](https://github.com/Eclipse-Laboratories-Inc/modular-network-component) component is a React TypeScript component designed to provide an easy toggle between different blockchain networks within a React application. It provides a tabbed interface that allows to easily navigate between various blockchain networks, and it dynamically connects to the selected blockchain using the associated RPC URL.

### Prerequisites[​](https://icarus131.github.io/devcookbook/docs/SwitchingChains#prerequisites) <a href="#prerequisites" id="prerequisites"></a>

* Ensure that your project has React installed. If not, you can create a new React project using Create React App.
* Copy the TabbedButtons.tsx file into your project directory.
* Import and use the TabbedButtons component in your desired React component or page.

```tsx
import TabbedButtons from "./TabbedButtons";

const App = () => {
  return (
    <div>
      <TabbedButtons tabs={customTabs} onTabClick={exampleFunction} />
    </div>
  );
};

export default App;
```

### Usage[​](https://icarus131.github.io/devcookbook/docs/SwitchingChains#usage) <a href="#usage" id="usage"></a>

The TabbedButtons component consists of a tabbed interface with predefined tabs for the Solana and Eclipse mainnet. You can easily switch between these networks by clicking on the respective tabs.

Customizing the tabs by overriding the default options can simply be done by creating a `customTabs` array. This array contains the user tab configurations. You can modify this array by adding, removing, or updating tab objects according to your requirements.

```rust
const customTabs = [
  {
    name: "Solana Devnet", //Example tabs that can be overriden by the user
    rpcUrl: "https://api.devnet.solana.com",
    iconSrc: "sol.png",
  },
  {
    name: "Eclipse Devnet",
    rpcUrl: "https://staging-rpc.dev.eclipsenetwork.xyz",
    iconSrc: "eclipse.jpg",
  },
];
```

### The onTabClick property[​](https://icarus131.github.io/devcookbook/docs/SwitchingChains#the-ontabclick-property) <a href="#the-ontabclick-property" id="the-ontabclick-property"></a>

You can extend the functionality of the TabbedButtons component by adding a custom function using the onTabClick property. This function is called each time a user switches between blockchain networks, and it receives the selected RPC URL as a parameter.

```rust
const exampleFunction = (rpcUrl: string, networkName: string) => {
  // Add your arbitrary function here that uses rpcUrl
  console.log(`Setting RPC URL: ${rpcUrl}`);
  // Example: creating alert to confirm the rpcUrl
  alert(
    `Executing custom function with RPC URL: ${rpcUrl} for network: ${networkName}`,
  );
};
```

### Customizing UI[​](https://icarus131.github.io/devcookbook/docs/SwitchingChains#customizing-ui) <a href="#customizing-ui" id="customizing-ui"></a>

The UI is designed to be easily modifiable. As for the icons, you can directly modify them using the customTabs array. For the styling of the tabs themselves, you can modify the TabbedButtons.tsx file to match the requirements of your project. Since the component uses a UI framework, all the styling is taken care of by just modifying class names. This allows for easy customization without any prior modification.

### Conclusion[​](https://icarus131.github.io/devcookbook/docs/SwitchingChains#conclusion) <a href="#conclusion" id="conclusion"></a>

The TabbedButtons component provides a user-friendly way to switch between blockchain networks in a React application. It is easily customizable, allowing developers to modify the UI, extend functionality and modify the displayed networks on the fly, according to their specific project requirements.
