# Add edeXa chain to metamask



MetaMask is a popular browser extension that allows users to interact with blockchain applications. This guide shows you how to integrate EDX into MetaMask using the edeXa mainnet



### Simple method <a href="#simple-method" id="simple-method"></a>

The easiest way to add a Filecoin testnet to your MetaMask is by using the pre-configured options at chainlist.org.

1. Go to [chainlist.org](https://chainlist.org/).
2. Enter edeXa into the search bar.
3. Scroll down to find the edeXa network.
4. In MetaMask click **Next**.
5. Click **Connect**.
6. Click **Approve** when prompted to _Allow this site to add a network_.
7. Click **Switch network** when prompted by MetaMask.
8. Open MetaMask from the browser extensions tab.
9. You should see edeXa listed at the top.

### Manual process <a href="#manual-process" id="manual-process"></a>

If you can’t or don’t want to use Chainlist, you can add the Filecoin network to your MetaMask manually.

#### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

Before we get started, you’ll need the following:

* A [Chromium-based browser](https://en.wikipedia.org/wiki/Chromium\_web\_browser#Browsers\_based\_on\_Chromium), or [Firefox](https://www.mozilla.org/en-CA/firefox/products/).
* A browser with [MetaMask](https://metamask.io/) installed.

#### Steps <a href="#steps" id="steps"></a>

The process for integrating edeXa into MetaMask is fairly simple but has some very specific variables that you must copy exactly.

1. Open your browser and open the MetaMask plugin. If you haven’t opened the MetaMask plugin before, you’ll be prompted to create a new wallet. Follow the prompts to create a wallet.
2.  Click the user circle and select **Settings**:

    ![Click Settings from within MetaMask](<.gitbook/assets/manual show settings\_hu959371a2f5a89b14fd41a7d9201d4d3f\_41332\_383x0\_resize\_q75\_h2\_box.webp>)
3. Select **Networks**.
4. Click **Add a network**.
5. Scroll down and click **Add a network manually**.
6.  Enter the following information into the fields:

    | Field           | Value                                                            |
    | --------------- | ---------------------------------------------------------------- |
    | Network name    | `edeXa`                                                          |
    | New RPC URL     | [`https://testnet.edexa.com/rpc`](https://testnet.edexa.com/rpc) |
    | Chain ID        | 1995                                                             |
    | Currency symbol | `EDX`                                                            |
7. Pick one a block explorers, and enter the URL into the **Block explorer (optional)** field.
8. Review the values in the fields and click **Save**.
9. The edeXa network should now be shown in your MetaMask window.
10. Done!
