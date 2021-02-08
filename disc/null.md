# Null
> Points: 50
> Category: Crypto

## Description
Have a look at this message I found in an old chest. I also found its key. ``key = [24, 28, 110, 126, 145, 200, 209, 222, 231, 332, 447, 460, 472, 476, 480, 569, 591, 629, 633, 650, 671, 695, 696, 699, 706, 833]``. Can you decrypt it? Don't forget to put the ``ictf{}`` wrapper around before submitting the flag.

## Attachments
[Github Link](https://github.com/ainzs-evil-twin/ictf-Feb-2021/blob/main/null/Corrosion.txt)

## Solution
Each element in the keys array looks like it can be used as an index...
```py
string = "CorrosionIsTheProcessThatCanDeteriorateTheQualityOfAMaterialOrChangeItToSomeOtherMaterialEntirelyDueToTheEffectsOfTheSurroundingsItCanGraduallyDestroyMaterialsByChemicalProcessesCausedNaturallyByTheEnvironmentItIsASignificantConcernForBiomaterialsToBeCorrosionResistantBecauseCorrosionCanCauseAMaterialToLoseItsOriginalFunctionalityAndBringAboutModificationsOnTheSurfaceItMightAlsoLeadToTheReleaseOfMetalIonsIntoTheBodyWhichCanResultInAllergicResponsesAndSometimesACarcinogenicResponseInWorstCaseScenariosCorrosionInAMetallicBiomaterialMayChangeThePhAndOxygenContentsOfTheSurroundingsAndAChangeInBehaviourMayOccurInSuchConditionsHenceThereShouldBeALimitToHowMuchCorrosionIsAllowedInAWorkingBiomaterialWeCanConcludeThatTheCorrosionProcessIsOneOfTheMostImportantMediatorsOfTheTissueResponseToMetallicBiomaterialsAndADecentUnderstandingAndAccurateEvaluationOfTheCorrosionBehaviourIsNecessaryInOrderToDesignBiomaterialsOftentimesNobleMetalsAndMetalsWhichDevelopInertOxideLayerOverTheirSurfacesAreSeenBeingUsedInMedicalDevicesWhereAlmostNoCorrosionIsTolerableOnTheOtherHandSometimesWeNeedBiomaterialsWhichAreResorbableAndDegradeGraduallyOverAPeriodOfTimeSoThatTheyCanBeReplacedWithNaturalTissuesInSuchCasesCorrosionIsANecessaryEvilAndCanBeUsedInOurFavour"

keys = [24, 28, 110, 126, 145, 200, 209, 222, 231, 332, 447, 460, 472, 476, 480, 569, 591, 629, 633, 650, 671, 695, 696, 699, 706, 833]

ans = ""
for i in keys:
    ans += string[i-1]
print(ans)
```
### Flag
> ictf{ancientcryptoisfascinating}
