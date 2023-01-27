# Integrating Router's Widget

We have developed a widget that can be used by other projects to give their users an option to perform cross-chain transactions directly from their UI.&#x20;

### Usage/Example

Router's widget can easily be integrated as an `iframe`. An example of the same is given below:

```javascript
  const baseUrl = "https://app.thevoyager.io/swap";

  const configuration = {
    isWidget: true,
    widgetId: "24", // get your unique widget id by contacting us on Telegram
    fromChain: "56",
    toChain: "137",
    fromToken: "0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56",
    toToken: "0x16ECCfDbb4eE1A85A33f3A9B21175Cd7Ae753dB4",
    dstChains: "137,56",
    dstTokens:
      "0x6855f7bb6287F94ddcC8915E37e73a3c9fEe5CF3,0x980111ae1B84E50222C8843e3A7a038F36Fecd2b",
    ctaColor: "#E8425A",
    textColor: "#1A1B1C",
    backgroundColor: "#3fb043",
    logoURI: "ipfs://QmaznB5PRhMC696u8yZuzN6Uwrnp7Zmfa5CydVUMvLJc9i/aDAI.svg",
  };

  const paramString = new URLSearchParams(configuration).toString();
  document.getElementById("widget__iframe").src = `${baseUrl}?${paramString}`;
```

{% hint style="info" %}
**Important Note:** \
To integrate the widget on your UI, you will be assigned a unique widget ID. To get your widget ID, please contact us on [Telegram](https://t.me/SurajChawla).
{% endhint %}

```html
  <iframe id="widget__iframe" height="610px" width="420px" 
  src="https://app.thevoyager.io/swap?isWidget=true&widgetId=widget-0101&fromChain=56&toChain=137&fromToken=0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56&toToken=0x16ECCfDbb4eE1A85A33f3A9B21175Cd7Ae753dB4"
  style="border: none; border-radius: 11px; box-shadow: 3px 3px 10px 4px rgba(0, 0, 0, 0.05);">
  </iframe>
```

Generate the **`paramString`** (as given in the example above) and attach it to the **`src`** of the **`iframe`**. Except for the **`isWidget`** parameter, all of the **`query params`** in the URL can be customized based on your requirement -

| Parameter         | Description                                                                                                                                                                                                                                                          |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `isWidget`        | `true` **(Required)**                                                                                                                                                                                                                                                |
| `widgetId`        | Unique for each widget **(Required)**                                                                                                                                                                                                                                |
| `fromChain`       | ChainId of the chain that needs to be shown as the default source chain. By default, the source chain will be chosen as the chain to which the user's wallet is connected. In case the user's wallet is not connected, Polygon is shown as the default source chain. |
| `toChain`         | ChainId of the chain that needs to be shown as the default destination chain. By default, BSC is shown as the destination chain.                                                                                                                                     |
| `fromToken`       | Address of the token that needs to be shown as the selected token on the source chain. By default, USDT will be shown as the source token.                                                                                                                           |
| `toToken`         | Address of the token that needs to be shown as the selected token on the destination chain. By default, USDT will be shown as the destination token.                                                                                                                 |
| `ctaColor`        | Color of the "Call to Action" buttons                                                                                                                                                                                                                                |
| `textColor`       | Color of all the text in the widget                                                                                                                                                                                                                                  |
| `backgroundColor` | Background color of the widget                                                                                                                                                                                                                                       |
| `logoURI`         | Circular logo URL - if not given, the original Router logo will be shown                                                                                                                                                                                             |

#### Restricting chains/tokens

There might also be a few cases in which a platform wants to show a selected list of chains or tokens for its users. With Router's widget, partners can do this by adding a few parameters. An example with restricted parameters -

```html
  <iframe height="610px" width="420px" 
  src="https://app.thevoyager.io/swap?isWidget=true&widgetId=widget-0101&fromChain=137&fromToken=0xc2132d05d31c914a87c6611c10748aeb04b58e8f&toChain=56&toToken=0x6855f7bb6287F94ddcC8915E37e73a3c9fEe5CF3&dstChains=137,56&dstTokens=0x6855f7bb6287F94ddcC8915E37e73a3c9fEe5CF3,0x980111ae1B84E50222C8843e3A7a038F36Fecd2b"
  style="border: none;border-radius: 11px;box-shadow: 3px 3px 10px 4px rgba(0, 0, 0, 0.05);">
  </iframe>
```

Restriction parameters are optional and can be added along with the aforementioned parameters as **`query params`** in the URL -

| Parameter   | Description                                                                                                                                                                                                   |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `srcChains` | List of chainIds separated by a comma that needs to be shown in the source chain selection menu.                                                                                                              |
| `dstChains` | List of chainIds separated by a comma that needs to be shown in the destination chain selection menu.                                                                                                         |
| `srcTokens` | List of token addresses belonging to the list of `srcChains` separated by a comma that needs to be shown in the source token selection menu.                                                                  |
| `dstTokens` | List of token addresses belonging to the list of `dstChains` separated by a comma that needs to be shown in the destination token selection menu.                                                             |
| `feeTokens` | List of addresses of fee tokens belonging to list of `srcChains` seperated by a comma that needs to be shown in the fee selection menu. Only these fee tokens will be available for the users to select from. |

**Important notes -**

1. **`height`** and **`width`** are customizable, but we recommend keeping the aspect ratio close to the default.
2. In the case of using restricted tokens along with the restricted chain parameter, at least one token address for each restricted chain should be provided. For example, if restricted chains on the source are chosen to be Polygon and BSC, and there is a restriction on tokens as well, then a minimum of 2 token addresses need to be specified in the restricted token parameter. One address for Polygon and another one for BSC. This will make sure that only one token is shown for Polygon and one for BSC for users to select.
