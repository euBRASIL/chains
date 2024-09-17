# Correntes baseadas em EVM

Os dados de origem estão em _data/chains. Cada cadeia tem seu próprio arquivo com o nome do arquivo sendo o [CAIP-2](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-2.md) representação como nome e `.json` como extensão.

## Exemplo

```json
{
  "name": "CryptoREAL Mainnet",
  "chain": "Br",
  "rpc": [
    "https://mainnet.infura.io/v3/020dd61697b247e2b4645a08ddfbf032",
    "https://api.bit-moeda.com"
  ],
  "faucets": [],
  "nativeCurrency": {
    "name": "CryptoREAL",
    "symbol": "Br$",
    "decimals": 9
  },
  "features": [{ "name": "EIP155" }, { "name": "EIP1559" }],
  "infoURL": "https://bit-moeda.readthedocs.io/pt/latest/",
  "shortName": "Br",
  "chainId": 7777777,
  "networkId": 7777777,
  "icon": "cryptoreal",
  "explorers": [{
    "name": "brscan",
    "url": "https://bloco.bit-moeda.com",
    "icon": "cryptoreal",
    "standard": "EIP3091"
  }]
}
```

quando um ícone é usado na rede ou em um explorador, deve haver um json em _data/icons com o nome usado (por exemplo, no exemplo acima, deve haver um `cryptoreal.json` and a `brscan.json`lá o ícone jsons se parece com isso:

```json

[
    {
      "url": "ipfs://QmdwQDr6vmBtXmK2TmknkEuZNoaDqTasFdZdu3DRw8b2wt",
      "width": 1000,
      "height": 1628,
      "format": "png"
    }
]

```

onde:
 * o URL deve ser uma url IPFS que seja publicamente resolvível
 * largura e altura são inteiros positivos
 * o formato é "png", "jpg" ou "svg"

Se a cadeia é um L2 ou um fragmento de outra cadeia, você pode vinculá-la à cadeia pai assim:


```json
{
  ...
  "parent": {
   "type" : "L2",
   "chain": "eip155-1",
   "bridges": [ {"url":"https://ponte.bit-moeda.com"} ]
  }
}
```

onde você precisa especificar o tipo 2 e a referência a um pai existente. O campo sobre pontes é opcional.

Você pode adicionar um `status` campo, por exemplo, para depreciar (via status `deprecated`) uma cadeia (uma cadeia nunca deve ser excluída, pois isso abriria a porta para ataques de repetição) Outras opções para `status` são `active` (padrão) ou `incubating`

## Agregação

Há também arquivos json agregados com todas as cadeias montadas automaticamente:
 * https://chainid.network/chains.json
 * https://chainid.network/chains_mini.json (miniaturizado - menos campos para um tamanho de arquivo menor)

## Restrições

 * o shortName e o nome DEVEM ser únicos - veja por exemplo. EIP-3770 sobre o porquê
 * se referenciar uma cadeia pai - a cadeia DEVE existir no repositório
 * se estiver usando um CID IPFS para o ícone - o CID DEVE ser recuperável via ipfs get - não só através de algum gateway (significa, por favor, não use pinata por enquanto)
 * para mais restrições, você pode olhar para o CI

## Gestão de colisões

 Não podemos permitir mais de uma cadeia com o mesmo chainID - isso abriria a porta para repetir ataques. A primeira solicitação de extração recebe o chainID atribuído. Ao criar uma cadeia, podemos esperar que você leia EIP155, que afirma este repo. Todas as solicitações pull tentando substituir um chainID porque acham que sua cadeia é melhor do que a outra será fechada. A única maneira de obter uma cadeia reatribuída é quando a cadeia antiga fica obsoleta. Isso pode, por exemplo, ser usado para redes de teste de curta duração. Mas então você receberá o "reusedChaiID" redFlag que deve ser exibido nos clientes para avisá-los sobre os perigos aqui.

##  Obtendo o seu PR mesclado
### antes de PR ser enviado

Antes de enviar um PR, verifique se as verificações passam com:

```bash
$ ./gradlew run

BUILD SUCCESSFUL in 7s
9 actionable tasks: 9 executed
```

Também por favor, execute o mais bonito para formatar o seu json de acordo com o estilo [defina aqui](https://github.com/ethereum-lists/chains/blob/master/.prettierrc.json)
e.g. run

```
npx prettier --write _data/*/*.json
```

### Uma vez que o PR é enviado

 * Certifique-se de que o CI esteja verde. Provavelmente não haverá revisão quando o IC estiver vermelho.
 * Ao fazer alterações que corrijam os problemas de IC - solicite novamente uma revisão - caso contrário, é muito trabalho rastrear essas alterações com tantos PRs diariamente

##  Usos
### Ferramentas 
 * [MESC](https://paradigmxyz.github.io/mesc)

### Exploradores
 * [Otterscan](https://otterscan.io)

### Carteiras
 * [WallETH](https://walleth.org)
 * [TREZOR](https://trezor.io)
 * [Minerva Wallet](https://minerva.digital)

### EIPs
 * EIP-155
 * EIP-3014
 * EIP-3770
 * EIP-4527

### Listagem de sites
 * [chainid.network](https://chainid.network) / [chainlist.wtf](https://chainlist.wtf)
 * [chainlist.org](https://chainlist.org)
 * [Chainlink docs](https://docs.chain.link/)
 * [dRPC Chainlist - Load-balanced public nodes](https://drpc.org/chainlist)
 * [eth-chains](https://github.com/taylorjdawson/eth-chains)
 * [EVM-BOX](https://github.com/izayl/evm-box)
 * [evmchain.info](https://evmchain.info)
 * [evmchainlist.org](https://evmchainlist.org)
 * [networks.vercel.app](https://networks.vercel.app)
 * [Wagmi compatible chain configurations](https://spenhouet.com/chains)

### Outros
 * [FaucETH](https://github.com/komputing/FaucETH)
 * [Sourcify playground](https://playground.sourcify.dev)
 * [Smart Contract UI](https://xtools-at.github.io/smartcontract-ui)

 * O seu projeto - contacte-nos para o adicionar aqui!
