<template>
  <div>
    <b-card
      border-variant="primary"
      header-bg-variant="primary"
      align="center"
    >
      <b-form-textarea
        id="input"
        v-model="inputData"
        placeholder="Input HTML data"
        rows="15"
        no-resize
      />
    </b-card>
    <b-button block squared variant="primary" @click="eraseHTML">
      変換
    </b-button>
    <b-card
      border-variant="primary"
      header-bg-variant="primary"
      align="center"
    >
      <b-form-textarea
        id="output"
        v-model="outputData"
        placeholder="Output HTML data"
        rows="15"
        no-resize
      />
    </b-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      inputData: '',
      outputData: ''
    }
  },
  methods: {
    eraseHTML() {
      let newData = ''
      const lCmt = '<!--'
      const rCmt = '-->'
      const bracketStack = []
      for (let i = 0; i <= this.inputData.length; i++) {
        const ch = this.inputData[i]
        if (ch === undefined) { continue }
        if (ch === '<') {
          // is comment "<!--"
          if (
            i + lCmt.length < this.inputData.length &&
            this.inputData.substr(i, lCmt.length) === lCmt
          ) {
            bracketStack.push('!')
          } else {
            bracketStack.push('<')
          }
          continue
        }
        if (ch === '>') {
          // is comment "-->"
          if (
            i - rCmt.length + 1 >= 0 &&
            this.inputData.substr(i - rCmt.length + 1, rCmt.length) === rCmt
          ) {
            while (
              bracketStack.length >= 1 &&
              bracketStack[bracketStack.length - 1] !== '!'
            ) {
              bracketStack.pop()
            }
            bracketStack.pop()
          } else {
            bracketStack.pop()
          }
          continue
        }

        if (bracketStack.length === 0) {
          newData += ch
        }
      }
      newData = this.narrowNewline(newData)
      newData = this.transChar(newData)
      this.outputData = newData
    },
    narrowNewline(str) {
      // TODO :
      const maxCnt = 2
      let res = ''
      const nl = '\n'
      let nlCnt = 0
      for (const ch of str) {
        if (ch === nl) {
          nlCnt++
          continue
        }
        for (let i = 0; i < Math.min(nlCnt, maxCnt); i++) {
          res += nl
        }
        nlCnt = 0
        res += ch
      }
      return res
    },
    transChar(str) {
      const chars = [
        ['&nbsp;', ' '],
        ['&cent;', '¢'],
        ['&pound;', '£'],
        ['&curren;', '¤'],
        ['&yen;', '¥'],
        ['&brvbar;', '¦'],
        ['&sect;', '§'],
        ['&uml;', '¨'],
        ['&copy;', '©'],
        ['&ordf;', 'ª'],
        ['&laquo;', '«'],
        ['&not;', '¬'],
        ['&shy;', '­'],
        ['&reg;', '®'],
        ['&macr;', '¯'],
        ['&deg;', '°'],
        ['&plusmn;', '±'],
        ['&sup2;', '²'],
        ['&sup3;', '³'],
        ['&acute;', '´'],
        ['&micro;', 'µ'],
        ['&para;', '¶'],
        ['&middot;', '·'],
        ['&cedil;', '¸'],
        ['&sup1;', '¹'],
        ['&ordm;', 'º'],
        ['&raquo;', '»'],
        ['&frac14;', '¼'],
        ['&frac12;', '½'],
        ['&frac34;', '¾'],
        ['&iquest;', '¿'],
        ['&Agrave;', 'À'],
        ['&Aacute;', 'Á'],
        ['&Acirc;', 'Â'],
        ['&Atilde;', 'Ã'],
        ['&Auml;', 'Ä'],
        ['&Aring;', 'Å'],
        ['&AElig;', 'Æ'],
        ['&Ccedil;', 'Ç'],
        ['&Egrave;', 'È'],
        ['&Eacute;', 'É'],
        ['&Ecirc;', 'Ê'],
        ['&Euml;', 'Ë'],
        ['&Igrave;', 'Ì'],
        ['&Iacute;', 'Í'],
        ['&Icirc;', 'Î'],
        ['&Iuml;', 'Ï'],
        ['&ETH;', 'Ð'],
        ['&Ntilde;', 'Ñ'],
        ['&Ograve;', 'Ò'],
        ['&Oacute;', 'Ó'],
        ['&Ocirc;', 'Ô'],
        ['&Otilde;', 'Õ'],
        ['&Ouml;', 'Ö'],
        ['&times;', '×'],
        ['&Oslash;', 'Ø'],
        ['&Ugrave;', 'Ù'],
        ['&Uacute;', 'Ú'],
        ['&Ucirc;', 'Û'],
        ['&Uuml;', 'Ü'],
        ['&Yacute;', 'Ý'],
        ['&THORN;', 'Þ'],
        ['&szlig;', 'ß'],
        ['&agrave;', 'à'],
        ['&aacute;', 'á'],
        ['&acirc;', 'â'],
        ['&atilde;', 'ã'],
        ['&auml;', 'ä'],
        ['&aring;', 'å'],
        ['&aelig;', 'æ'],
        ['&ccedil;', 'ç'],
        ['&egrave;', 'è'],
        ['&eacute;', 'é'],
        ['&ecirc;', 'ê'],
        ['&euml;', 'ë'],
        ['&igrave;', 'ì'],
        ['&iacute;', 'í'],
        ['&icirc;', 'î'],
        ['&iuml;', 'ï'],
        ['&eth;', 'ð'],
        ['&ntilde;', 'ñ'],
        ['&ograve;', 'ò'],
        ['&oacute;', 'ó'],
        ['&ocirc;', 'ô'],
        ['&otilde;', 'õ'],
        ['&ouml;', 'ö'],
        ['&divide;', '÷'],
        ['&oslash;', 'ø'],
        ['&ugrave;', 'ù'],
        ['&uacute;', 'ú'],
        ['&ucirc;', 'û'],
        ['&uuml;', 'ü'],
        ['&yacute;', 'ý'],
        ['&thorn;', 'þ'],
        ['&yuml;', 'ÿ'],
        ['&fnof;', 'ƒ'],
        ['&Alpha;', 'Α'],
        ['&Beta;', 'Β'],
        ['&Gamma;', 'Γ'],
        ['&Delta;', 'Δ'],
        ['&Epsilon;', 'Ε'],
        ['&Zeta;', 'Ζ'],
        ['&Eta;', 'Η'],
        ['&Theta;', 'Θ'],
        ['&Iota;', 'Ι'],
        ['&Kappa;', 'Κ'],
        ['&Lambda;', 'Λ'],
        ['&Mu;', 'Μ'],
        ['&Nu;', 'Ν'],
        ['&Xi;', 'Ξ'],
        ['&Omicron;', 'Ο'],
        ['&Pi;', 'Π'],
        ['&Rho;', 'Ρ'],
        ['&Sigma;', 'Σ'],
        ['&Tau;', 'Τ'],
        ['&Upsilon;', 'Υ'],
        ['&Phi;', 'Φ'],
        ['&Chi;', 'Χ'],
        ['&Psi;', 'Ψ'],
        ['&Omega;', 'Ω'],
        ['&alpha;', 'α'],
        ['&beta;', 'β'],
        ['&gamma;', 'γ'],
        ['&delta;', 'δ'],
        ['&epsilon;', 'ε'],
        ['&zeta;', 'ζ'],
        ['&eta;', 'η'],
        ['&theta;', 'θ'],
        ['&iota;', 'ι'],
        ['&kappa;', 'κ'],
        ['&lambda;', 'λ'],
        ['&mu;', 'μ'],
        ['&nu;', 'ν'],
        ['&xi;', 'ξ'],
        ['&omicron;', 'ο'],
        ['&pi;', 'π'],
        ['&rho;', 'ρ'],
        ['&sigmaf;', 'ς'],
        ['&sigma;', 'σ'],
        ['&tau;', 'τ'],
        ['&upsilon;', 'υ'],
        ['&phi;', 'φ'],
        ['&chi;', 'χ'],
        ['&psi;', 'ψ'],
        ['&omega;', 'ω'],
        ['&thetasym;', 'ϑ'],
        ['&upsih;', 'ϒ'],
        ['&piv;', 'ϖ'],
        ['&bull;', '•'],
        ['&hellip;', '…'],
        ['&prime;', '′'],
        ['&Prime;', '″'],
        ['&oline;', '‾'],
        ['&frasl;', '⁄'],
        ['&weierp;', '℘'],
        ['&image;', 'ℑ'],
        ['&real;', 'ℜ'],
        ['&trade;', '™'],
        ['&alefsym;', 'ℵ'],
        ['&larr;', '←'],
        ['&uarr;', '↑'],
        ['&rarr;', '→'],
        ['&darr;', '↓'],
        ['&harr;', '↔'],
        ['&crarr;', '↵'],
        ['&lArr;', '⇐'],
        ['&uArr;', '⇑'],
        ['&rArr;', '⇒'],
        ['&dArr;', '⇓'],
        ['&hArr;', '⇔'],
        ['&forall;', '∀'],
        ['&part;', '∂'],
        ['&exist;', '∃'],
        ['&empty;', '∅'],
        ['&nabla;', '∇'],
        ['&isin;', '∈'],
        ['&notin;', '∉'],
        ['&ni;', '∋'],
        ['&prod;', '∏'],
        ['&sum;', '∑'],
        ['&minus;', '−'],
        ['&lowast;', '∗'],
        ['&radic;', '√'],
        ['&prop;', '∝'],
        ['&infin;', '∞'],
        ['&ang;', '∠'],
        ['&and;', '∧'],
        ['&or;', '∨'],
        ['&cap;', '∩'],
        ['&cup;', '∪'],
        ['&int;', '∫'],
        ['&there4;', '∴'],
        ['&sim;', '∼'],
        ['&cong;', '≅'],
        ['&asymp;', '≈'],
        ['&ne;', '≠'],
        ['&equiv;', '≡'],
        ['&le;', '≤'],
        ['&ge;', '≥'],
        ['&sub;', '⊂'],
        ['&sup;', '⊃'],
        ['&nsub;', '⊄'],
        ['&sube;', '⊆'],
        ['&supe;', '⊇'],
        ['&oplus;', '⊕'],
        ['&otimes;', '⊗'],
        ['&perp;', '⊥'],
        ['&sdot;', '⋅'],
        ['&lceil;', '⌈'],
        ['&rceil;', '⌉'],
        ['&lfloor;', '⌊'],
        ['&rfloor;', '⌋'],
        ['&lang;', '〈'],
        ['&rang;', '〉'],
        ['&loz;', '◊'],
        ['&spades;', '♠'],
        ['&clubs;', '♣'],
        ['&hearts;', '♥'],
        ['&diams;', '♦'],
        ['&quot;', '"'],
        ['&amp;', '&'],
        ['&lt;', '<'],
        ['&gt;', '>'],
        ['&OElig;', 'Œ'],
        ['&oelig;', 'œ'],
        ['&Scaron;', 'Š'],
        ['&scaron;', 'š'],
        ['&Yuml;', 'Ÿ'],
        ['&circ;', 'ˆ'],
        ['&tilde;', '˜'],
        ['&ensp;', ' '],
        ['&emsp;', ' '],
        ['&thinsp;', ' '],
        ['&ndash;', '–'],
        ['&mdash;', '—'],
        ['&lsquo;', '‘'],
        ['&rsquo;', '’'],
        ['&sbquo;', '‚'],
        ['&ldquo;', '“'],
        ['&rdquo;', '”'],
        ['&bdquo;', '„'],
        ['&dagger;', '†'],
        ['&Dagger;', '‡'],
        ['&permil;', '‰'],
        ['&lsaquo;', '‹'],
        ['&rsaquo;', '›'],
        ['&euro;', '€']
      ]

      let res = ''
      for (let i = 0; i < str.length; i++) {
        if (str[i] !== '&') {
          res += str[i]
          continue
        }

        let found = false
        for (const pair of chars) {
          const key = pair[0]
          const val = pair[1]
          if (i + key.length - 1 < str.length && str.substr(i, key.length) === key) {
            res += val
            found = true
            i += key.length - 1
            break
          }
        }

        if (!found) {
          res += '&'
        }
      }
      return res
    }
  }
}
</script>
