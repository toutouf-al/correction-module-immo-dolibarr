# correction-module-immo-dolibarr
Proposition de correction du module immobilisation de dolibarr V22.02

Je précise que l'origine de ces bugs ont été trouvé avec l'aide d'une IA, je laisse le soin au dévelopeur du module de vérifié que ces corrections sont compatible avec le reste du code !

Les problèmes que j'ai rencontré en v22.02 :

- Les écriture de dépréciation sont passé à l'envers, on crédite le compte 68112 et on débite le compte 2815, c'est l'inverse normalement.
    Proposition solution --> htdocs\asset\class\assetaccountancycodes.class.php
    Ligne 71 et 72 :

                                    //Avant :
                                  'depreciation_debit' => 'depreciation_asset',
                              			'depreciation_credit' => 'depreciation_expense',
                              
                                      //Après :
                              			'depreciation_debit' => 'depreciation_expense',
                              			'depreciation_credit' => 'depreciation_asset',
                              
- Lorsque l'on veut passer les ecriture d'ammortissement le module veut passer systématiquement des ecriture d'amortissement dérogatoire alors que la case dans l'immo est bien décochée.
  Proposition solution --> htdocs\asset\class\assetdepreciationoptions.class.php
  Ligne 318 :
  
                                      // AVANT
                                    if (isset($deprecation_options[$info[0]][$info[1]]) && $deprecation_options[$info[0]][$info[1]] != $info[2]) {
                                        unset($deprecation_options[$mode_key]);
                                    }
                                    
                                    // APRÈS  
                                    if (!isset($deprecation_options[$info[0]][$info[1]]) || $deprecation_options[$info[0]][$info[1]] != $info[2]) {
                                        unset($deprecation_options[$mode_key]);
                                    }
