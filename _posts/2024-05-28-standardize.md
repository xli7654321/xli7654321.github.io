---
layout: post
title: Standardizing SMILES Using RDKit
date: 2024-05-28 12:00
description: "\"Garbage in, Garbage out\""
tags: RDKit
giscus_comments: true
featured: true
---

<style>
h1,
h2,
h3,
h4,
h5,
h6 {
  color: var(--global-theme-color);
}

.post-title {
  font-weight: 500;
}
</style>

{% highlight python linenos %}

from rdkit import Chem
from rdkit.Chem.MolStandardize import rdMolStandardize

def standardize(smiles: str, stereo=False, canon_tautomer=True) -> str:
    """Standardizing SMILES Using RDKit.

    Following: 
    1. https://bitsilla.com/blog/2021/06/standardizing-a-molecule-using-rdkit/
    2. https://github.com/greglandrum/RSC_OpenScience_Standardization_202104/blob/main/MolStandardize%20pieces.ipynb
    3. https://www.youtube.com/watch?v=eWTApNX8dJQ
    4. https://www.blopig.com/blog/2022/05/molecular-standardization/

    :param smiles: SMILES string.
    :param stereo: If True, keep stereochemistry information.
    :param canon_tautomer: If True, canonicalize tautomer.
    :returns: Standardized SMILES string.
    """
    try:
        mol = Chem.MolFromSmiles(smiles)

        # rdMolStandardize.Cleanup(mol) is equivalent to the molvs.Standardizer().standardize(mol) function
        # Remove H atoms, Sanitize, Disconnect metal bonds, Normalize, Reionize, Assign stereochemistry
        clean_mol = rdMolStandardize.Cleanup(mol)

        # Get the actual mol we are interested in (e.g., remove mixtures or salts, etc.)
        clean_mol = rdMolStandardize.FragmentParent(clean_mol)

        # Neutralize
        uncharger = rdMolStandardize.Uncharger()
        clean_mol = uncharger.uncharge(clean_mol)

        # Remove isotope
        for atom in clean_mol.GetAtoms():
            atom.SetIsotope(0)
    
        # Remove stereochemistry
        if not stereo:
            Chem.RemoveStereochemistry(clean_mol)

        # Get the canonical tautomer
        if canon_tautomer:
            te = rdMolStandardize.TautomerEnumerator()
            clean_mol = te.Canonicalize(clean_mol)

        return Chem.MolToSmiles(clean_mol)
    
    except Exception as e:
        print(f"{smiles}; {e}")
        # Return the original SMILES for further inspection or return None
        return smiles

{% endhighlight %}
