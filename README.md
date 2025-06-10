import React, { useState } from 'react';
import Web3 from 'web3';
import './App.css';

const App = () => {
  const [account, setAccount] = useState('');
  const [balance, setBalance] = useState(0);
  const tokens = [
    { id: 'ACT_100K', amount: '100 000 ACT', price: '100 €', yield: '10-15%/an' },
    { id: 'ACT_1M', amount: '1 000 000 ACT', price: '1000 €', yield: '10-15%/an' },
    { id: 'ACT_5M', amount: '5 000 000 ACT', price: '5000 €', yield: '10-15%/an' },
  ];

  const connectWallet = async () => {
    if (window.ethereum) {
      const web3 = new Web3(window.ethereum);
      await window.ethereum.enable();
      const accounts = await web3.eth.getAccounts();
      setAccount(accounts[0]);
      setBalance(1000000); // Solde fictif : 1M ACT
    } else {
      alert('Installez Metamask pour payer en ACT/USDT');
    }
  };

  const buyWithCrypto = async (tokenId, price) => {
    alert(`Achat de ${tokenId} avec ACT/USDT pour ${price}`);
  };

  const buyWithFiat = (tokenId, price) => {
    alert(`Redirection vers Stripe pour ${tokenId} - ${price}`);
  };

  return (
    <div className="App">
      <header>
        <h1>Kems</h1>
        <p>Investissez dans l’agriculture avec AgriCoopToken (ACT)</p>
      </header>
      <button className="connect-btn" onClick={connectWallet}>
        {account ? `Connecté : ${account.slice(0, 6)}...` : 'Connecter Metamask'}
      </button>
      {account && <p>Solde : {balance} ACT ({(balance * 0.001).toFixed(2)} €)</p>}
      <section>
        <h2>Projet : Élevage de 100 poules bio</h2>
        <p>Objectif : 10 000 € (10M ACT) | Collecté : 0 €</p>
        <p>Rendement estimé : 10-15 %/an</p>
        <a href="https://drive.google.com/file/..." target="_blank" rel="noopener noreferrer">
          Lire le whitepaper
        </a>
      </section>
      <section>
        <h3>Achetez des jetons ACT</h3>
        <div className="token-list">
          {tokens.map(token => (
            <div key={token.id} className="token">
              <h4>{token.amount}</h4>
              <p>Prix : {token.price}</p>
              <p>Rendement : {token.yield}</p>
              <button onClick={() => buyWithCrypto(token.id, token.price)}>Payer en ACT/USDT</button>
              <button onClick={() => buyWithFiat(token.id, token.price)}>Payer en EUR</button>
            </div>
          ))}
        </div>
      </section>
      <section>
        <h3>Tableau de bord</h3>
        <p>Investissements : Aucun (simulation).</p>
        <p>Dividendes estimés : 0 € (10-15 %/an après achat).</p>
      </section>
      <footer>
        <p>© 2025 Kems. Tous droits réservés.</p>
      </footer>
    </div>
  );
};

export default App;

