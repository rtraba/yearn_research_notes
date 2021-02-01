
check who is the keeper

report  functions

_assessFees function

https://feel-the-yearn.app/vaults
https://yvault-roi.netlify.app/
https://yieldfarming.info/yearn/yvault/

https://github.com/lehnberg/yearn-diagrams



Everything starts with keepers calling to harvest, the frecuency and requirements taht keepers should meet depends on jobs created on keeper network at the begining. 

Who cna call to harvest?- 

what harvest does?: Harvests the Strategy, recognizing any profits or losses and adjusting the Strategy's position.

who can execute harvest?: This may only be called by governance, the strategist, or the keeper

This is key to undertand the diferent scenariosscenarios: 
When `harvest()` is called, the Strategy reports to the Vault (via `vault.report()`), so in some cases `harvest()` must be called in order to take in profits, to borrow newly available funds from the Vault, or otherwise adjust its position. In other cases `harvest()` must be called to report to the Vault on the Strategy's position, especially if any losses have occurred.


the firs important thing that harvest makes is this, before calling to report: (profit, loss, debtPayment) = prepareReturn(vault.debtOutstanding());



key events to check fees, gains, debts): 
harvested on strategy, 
StrategyReported (index_topic_1 address strategy, uint256 gain, uint256 loss, uint256 totalGain, uint256 totalLoss, uint256 totalDebt, uint256 debtAdded, uint256 debtLimit)


fees structures and flows:
governance_fee
strategist_fee
total_fee = governance_fee + strategist_fee
Strategy distributes rewards at the end of harvest!!!!!



Assess fees receive two parmeter: gains (also colled propfit, and address of the respecting strategy)

prepareReturn sets profit being the (psitive) diference between strategy's balance of dai and the current ammount of debt in weth.

!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
assesFees makes 3 importantan calculations:
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
GOVERNANCE 

 governance_fee: uint256 = (
        (self.receivertalAssets() /*self.token.balanceOf(self) + self.totalDebt*/ * (block.timestamp - self.lastReport) * self.managementFee)
        / FEE_MAX                  /* 10_000 */
        / SECS_PER_YEAR          /*  31_557_600 */
    )

 how this diference is mesured in timestamp???? unis standar time seconds.

STRATEGIST 
if gain > 0:
        # NOTE: Unlikely to throw unless strategy reports >1e72 harvest profit
        strategist_fee = (
            gain * self.strategies[strategy].performanceFee   /* */
        ) / FEE_MAX
        # NOTE: Unlikely to throw unless strategy reports >1e72 harvest profit
        governance_fee += gain * self.performanceFee / FEE_MAX      /* this line says govfee = govfee+stratfee */

