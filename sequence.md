
sequenceDiagram
  Kepper->>+Strategy: harvest()
  #check if the strtegy has outpassed debt its limit
  Strategy->>+Vault: debtOutstanding()
  Strategy->>+Strategy: prepareReturn(): profit, loss, debtPayment
  Strategy->>+Vault: report()
  #Vault->>Vault: _reportLoss()
  Vault->>Vault: self._assessFees(msg.sender, gain)
  Vault-->>-Strategy: _

  Strategy->>-Strategy: distributeRewards()
  
  Strategy->>Strategy: adjustPosition(debtOutstanding)
  Strategy->>Strategy: emit Harvested(profit, loss, debtPayment, debtOutstanding)

  Vault->>-Strategy: finish()
  Strategy->>-Strategy: finish()