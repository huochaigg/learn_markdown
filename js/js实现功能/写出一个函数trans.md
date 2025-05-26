 写出一个函数trans，将数字转换成汉语的输出，输入为不超过10000亿的数字
 trans(123456) —— 十二万三千四百五十六
 trans（100010001）—— 一亿零一万零一
```
function trans(num) {
      const numStr = String(num).split('')
      const numArr = ['零', '一', '二', '三', '四', '五', '六', '七', '八', '九']
      const unitArr = ['', '十', '百', '千', '万', '亿']

      let result = ''
      let zeroCount = 0

      for (let i = 0; i < numStr.length; i++) {
        const digit = numStr[i]
        const unitIndex = numStr.length - i - 1
        const unit = unitArr[unitIndex % 4]

        if (digit !== '0') {
          if (zeroCount > 0) {
            result += numArr[0]
            zeroCount = 0
          }
          result += numArr[digit] + unit
        } else {
          zeroCount++
        }

        if (unitIndex % 4 === 0 && zeroCount > 0) {
          result += unitArr[Math.floor(unitIndex / 4) + 4]
        }
      }

      return result.replace(/零+/g, '零').replace(/零$/, '').replace(/亿零$/, '亿')
    }
```