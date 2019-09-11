
## Big Data Questions 

* Top K problems

* Data structure: bloom filter 

* Reservoir sampling to randomly select `k` elements from a large number of `n` files  

544 - Top k Largest Numbers

Given an integer array, find the top k largest numbers in it.

Input: 

[3, 10, 1000, -99, 4, 100] and k = 3

Output: 

[1000, 100, 10]
    


```python
# Python 1: use partition, similar to quick select 
# move all elements bigger than a pivot to front, and check the pivot element location

class Solution:
    """
    @param nums: an integer array
    @param k: An integer
    @return: the top k largest numbers in array
    """
    def topk(self, nums, k):
        if len(nums) <= k:
            return nums 
        results = set()
        self.quick_select(nums, 0, len(nums) - 1, k, results)
        results = list(results)
        results.sort()
        results.reverse()
        return results
    
    def quick_select(self, nums, start, end, k, results):
        if start >= end:
            return 
        
        left, right = start, end 
        pivot = nums[(start + end) // 2]
        while left <= right:
            while left <= right and nums[left] > pivot:
                left += 1 
            while left <= right and nums[right] < pivot:
                right -= 1 
            if left <= right:
                nums[left], nums[right] = nums[right], nums[left]
                left += 1 
                right -= 1 
        # now left >= right  
        print('left', left)
        print('right', right)
        if k > left:
            for i in range(0, right + 1):
                results.add(nums[i])
            self.quick_select(nums, left, end, k - right, results)
        if k == left:
            for i in range(0, right + 1):
                results.add(nums[i])
        if k < left:
            self.quick_select(nums, start, right, k, results)
    
```


```python
# Python 2: as in notebook: Data structure: stack, queue, hash and heap, use heap 
import heapq
class Solution:
    """
    @param nums: an integer array
    @param k: An integer
    @return: the top k largest numbers in array
    """
    def topk(self, nums, k):
        if len(nums) == 0:
            return []
        heap = []
        for i in range(len(nums)):
            heapq.heappush(heap, nums[i])
            if len(heap) > k:
                heapq.heappop(heap)
        res = []
        while len(heap) > 0:
            res.append(heapq.heappop(heap))
           # print(res)
        res.reverse()
        return res
    
```

545 - Top k Largest Numbers II

Implement a data structure, provide two interfaces:

`add(number)`: Add a new number in the data structure.

`topk()`: Return the top k largest numbers in this data structure. k is given when we create the data structure.

Example: 

Input:

s = new Solution(3);

s.add(3)

s.add(10)

s.topk()

s.add(1000)

s.add(-99)

s.topk()

s.add(4)

s.topk()

s.add(100)

s.topk()
		
Output: 

[10, 3]

[1000, 10, 3]

[1000, 10, 4]

[1000, 100, 10]




```python
from heapq import heappush, heappop 
class Solution:
    """
    @param: k: An integer
    """
    def __init__(self, k):
        self.heap = []
        self.k = k

    """
    @param: num: Number to be added
    @return: nothing
    """
    def add(self, num):
        heappush(self.heap, num)
        print(self.heap)
        if len(self.heap) > self.k:
            heappop(self.heap)
            print('poped', self.heap)
        
        

    """
    @return: Top k element
    """
    def topk(self):
        # use sorted instead of sort! sort will sort in-place, thus change the heap structure
        return sorted(self.heap, reverse = True) 
```


```python
p = Solution(3)
p.add(3)
p.add(10)
p.topk()
p.add(1000)
p.add(-99)
p.topk()
p.add(4)
p.topk()
p.add(100)
p.topk()
```

    [3]
    [3, 10]
    [3, 10, 1000]
    [-99, 3, 1000, 10]
    poped [3, 10, 1000]
    [3, 4, 1000, 10]
    poped [4, 10, 1000]
    [4, 10, 1000, 100]
    poped [10, 100, 1000]





    [1000, 100, 10]



471 - Top K Frequent Words

Given a list of words and an integer `k`, return the top k frequent words in the list.


* Note: 

*You should order the words by the frequency of them in the return list, the most frequent one comes first. If two words has the same frequency, the one with lower alphabetical order come first.*



Example: 

Input:

  [
    "yes", "lint", "code",
    
    "yes", "code", "baby",
    
    "you", "baby", "chrome",
    
    "safari", "lint", "code",
    
    "body", "lint", "code"
  ]
  
  k = 3
  
Output: 

["code", "lint", "baby"]

* Hash table, heap 

* Offline algorithm



```python
# Python 1: use hash table and heap, this is a simplified version that doesn't satisfy 
# "If two words has the same frequency, the one with lower alphabetical order come first." and the ouput is 
# not ordered 

from heapq import heappush, heappop, nsmallest
class Solution:
    """
    @param words: an array of string
    @param k: An integer
    @return: an array of string
    """
    def topKFrequentWords(self, words, k):
        if k == 0:
            return []
        # count the frequency of each word, O(N) time and space  
        hash_table = {}
        for word in words:
            hash_table.setdefault(word, 0)
            hash_table[word] += 1
        print(hash_table)   
        # convert to top K problem on the frequency values, O(NlogK) time
        heap = []
        for word in hash_table:
            heappush(heap, (hash_table[word], word))
            if len(heap) > k:
                heappop(heap)
        # unordered output         
        return [heap[i][1] for i in range(len(heap))]

```


```python
Solution().topKFrequentWords([ "yes", "lint", "code",

"yes", "code", "baby",

"you", "baby", "chrome",

"safari", "lint", "code",

"body", "lint", "code"
], 3)
```

    {'yes': 2, 'lint': 3, 'code': 4, 'baby': 2, 'you': 1, 'chrome': 1, 'safari': 1, 'body': 1}





    ['yes', 'lint', 'code']




```python
# Python 2: use hash table and heap, take the order info into account 
from heapq import heappush, heappop, nsmallest
class Solution:
    """
    @param words: an array of string
    @param k: An integer
    @return: an array of string
    """
    def topKFrequentWords(self, words, k):
        if k == 0:
            return []
        # count the frequency of each word, O(N) time and space  
        words.sort(reverse = True)
        index = 0 
        
        hash_table = {}
        for word in words:
            hash_table.setdefault(word, 0)
            hash_table[word] += 1
#         print(hash_table)   
        
        # convert to top K problem on the frequency values, O(NlogK) time
        heap = []
        for word in hash_table:
            heappush(heap, (hash_table[word], words.index(word), word))
            if len(heap) > k:
                heappop(heap)
#         return [heap[i][2] for i in range(len(heap))]
         # reorder the output so the highest frequency comes first 
        heap.sort(reverse = True)
        return [heap[i][2] for i in range(len(heap))]


       
            
        
        

```


```python
Solution().topKFrequentWords([ "yes", "lint", "code",

"yes", "code", "baby",

"you", "baby", "chrome",

"safari", "lint", "code",

"body", "lint", "code"
], 3)
```




    ['code', 'lint', 'baby']




```python
# Solution().topKFrequentWords(["fayw","hb","yxbq","yw","bcvy","qin","tn","vw","whnp","bbq","icbb","yv","jp","cxv","bg","wm","gzc","jqzh","tg","cq","mqc","pa","mk","bypk","wipf","mfc","mvqi","njc","nhhb","dpt","wpd","cy","bzqw","bqbh","gy","hp","gyx","hqw","pkhw","bdac","bi","bj","wt","ij","mh","jb","fkh","af","tfqd","nyq","cb","fwc","mbtw","kn","gwzi","mj","abpb","wb","xwh","mhyw","gfa","izwk","cym","pz","hj","wqgw","dgmy","hwh","hbpb","gx","pxc","hnya","afbc","qxhc","cjba","cfva","dbqp","cnm","hj","pkbw","yw","mak","cina","zcd","cd","aytj","cxfn","tv","hf","nyh","kbw","ckx","wa","bbt","if","jcwn","qw","wx","cdw","qpbv","bb","hz","dxbt","yv","gb","xhyv","jyc","qbx","whj","cchd","dkqn","agxn","chj","ktfb","jvg","wg","qbyg","phjk","jzp","cfhd","pq","qcb","pkwq","dh","bwy","hdyq","ctgm","fh","hwc","tcq","acig","jqhn","tb","batd","ht","vkb","jwdf","wgb","chd","xc","mqdz","fjia","wht","bfw","bjyb","kyph","dbbz","wcac","bb","fcim","yzyw","dmhb","dmiw","wti","ibb","dcw","vdzw","bpbj","wpwc","zbaf","fiv","pma","hwy","yq","xb","zjbg","ac","wvb","fvac","zq","ja","fxjd","zih","jzp","qjwt","gb","wp","gvfp","cgyc","pz","tw","bizq","my","mwt","zn","jxw","wwik","hb","bdgh","mtk","jvwq","bh","hjv","zqj","wq","zjca","hchw","kc","yamy","yv","bmg","ygkf","an","acjx","ic","hgf","yhcz","zy","bqhb","hww","dyb","hpvc","nzwv","bgbq","bhbb","ww","yg","mbbi","baic","nvt","nbc","axbi","jbap","gdw","cwcy","bx","nc","qm","yc","zdi","gxp","bnj","pvq","pn","qb","hiwc","pdbc","hdzy","ch","yb","bwk","qbj","bamj","wynh","bjh","vw","ip","xay","xdcq","bb","axzt","jyq","bvx","nhty","xt","bh","awx","zv","tabi","ywiq","wyqh","ch","jxnz","in","bxtw","bw","acpw","gtb","bhw","win","bc","yhng","dywh","wygc","mgcw","ywk","avhj","ihvb","dgpi","bwta","hct","tmw","byd","hd","cxd","gxhc","qngb","hfa","fby","whfq","twc","gx","igb","mc","atm","khw","wk","nx","cy","wm","yv","iv","kd","phh","ib","ky","fh","ixfj","nv","bb","qp","fpw","wgk","htbj","ybab","ia","dkwz","nvh","wcm","yw","bfj","kb","dpw","dc","bwxh","th","fi","hbxd","wak","nt","zwqv","yvfg","xdb","fz","cmij","yjx","yc","gt","zat","zwfy","bv","by","pz","wy","fb","jhq","bwtk","whdh","vt","pqdz","nbji","fxb","ja","tb","xkqy","qbgj","dmf","td","wh","cj","bxyh","kn","gw","xz","bd","avd","in","pa","bdby","zga","jykh","cqkh","bp","yz","wdn","nq","xfqk","zbk","wz","hpc","ibng","nc","bcxd","yb","yb","dwp","yvak","fwh","bfac","ch","vfjc","jxci","bj","vb","mwpn","xfc","wab","hbwq","cyh","vhhy","yhw","mh","zdfc","ih","bh","hyq","bvcb","mfcd","gb","fcpz","czx","bqbz","ybjm","ji","mbd","wyca","pghc","yw","bt","bh","vw","qz","bbv","bcg","xc","ic","wf","jqmg","xbf","dkw","cqjc","wzij","hwck","wf","znat","xg","hw","xchc","bg","btda","yvp","baji","pjbw","hkdg","zcb","ccb","bmhp","bcng","gzt","tb","nw","py","vt","yyfk","ft","cwbi","kc","cq","mfxq","hch","wk","kmh","wbh","pkw","wgwa","vbw","cfd","cid","dbn","iw","hc","tc","afmv","mhay","zhb","bawk","camh","pb","tfcj","wcb","ica","cdjb","xhb","ia","nwx","iy","vgmh","wwcc","pyy","bxbd","bpyd","pgb","yixa","kwn","dby","gyda","jdt","mz","vcpf","bq","qb","zd","nzf","wtj","it","bmy","hbc","ca","gtmy","qw","bgxf","pqzv","chtf","mc","wkfp","ihk","wq","zgmk","gdbb","mq","gc","wy","by","ftgz","hym","gwy","ywak","ymd","jnf","hcc","yy","acyz","zka","bnfk","cgvm","qakd","bdvc","db","cxg","bc","wcp","tn","wcg","gypz","nkhv","wg","fm","tc","zc","db","dwbz","vy","ikb","bqdm","bdbh","vwip","wmf","km","dkca","wd","dkcy","zwh","hvd","pnbz","wdc","mqn","ygw","wqn","vwkc","fw","bxw","bfdv","yjft","nq","jbcw","gbiy","vdp","yjq","amdk","jbw","jmi","fm","ikq","aht","abh","dbgv","gapv","jv","ign","thn","apc","qcym","bnw","ccf","jv","bbj","fy","yn","ahv","mvtf","gm","qv","tbad","hya","zfhn","izyt","thy","fy","xhhp","hb","vqhn","iy","twbk","bwhh","qmtw","bzx","jb","bf","jhmk","myz","bcy","bc","hykg","bkd","ji","vwtj","bf","cvxi","nx","pyyn","zqyb","xzbb","hy","mnx","daz","fgkv","wg","tmj","xqwc","ycvq","hqb","bbfp","phw","ynyz","pbdh","gcbn","ab","yy","cbb","cgqi","wgc","vmwz","gpm","ghy","hy","qbc","chap","bqh","yyft","atqj","fww","jc","tcb","jiq","mhf","fcc","nwvp","gq","xqat","cfv","hgmx","igph","finj","ywp","cf","qab","jd","gp","pm","dy","ky","mjy","hn","zn","hbny","wqb","ybkf","pfx","xbap","yyw","at","cb","bh","ycqz","wh","qcyk","cyw","yjch","cy","pn","gbhz","zght","jdcv","qnc","nhbi","wb","chp","nhv","fx","gp","th","thi","iy","jhnh","fn","vba","xgym","bxy","hib","bh","apj","bw","yxvj","mi","nb","gyh","cyh","yqpj","jpwb","ncwh","bpjz","tpy","mnp","aytg","gxh","yz","dby","xzj","qmt","ykg","qnwf","nc","kbw","wcyh","bf","nh","jnw","bjz","fz","ta","yjy","tq","pv","zjkv","wh","dytj","tifz","vwz","btc","xwcc","bnxc","wbt","gbbn","bj","cbxh","bnjy","bpkc","qh","vw","gv","kp","gmqw","fqtg","wj","vpmh","yg","hyag","chwz","jhdk","kfhw","gx","qah","cthb","qihh","nbk","zydh","nmb","kw","dp","wjv","jd","ytxh","ivqt","za","iwcc","fy","cwwp","ic","yk","wxfa","ykq","qbm","qzy","hc","bbhy","bjfc","ybp","thv","bvc","kcxb","dhzw","yxj","kgwz","xhi","yqpz","hzq","zyby","xan","bq","waky","cy","qt","pkz","ncvz","yp","tpb","ahf","cn","yhc","fb","wfnv","ib","dab","yiqj","wqjw","jafx","jn","qipj","bfa","pv","hiwk","wab","mw","gpx","kj","qn","pqv","bfk","kayg","bwza","wbbw","bhk","pwhi","bdym","jyht","mwaf","djt","wm","ph","zayh","wzvh","zkm","hzb","nbyd","hvyz","mqfc","pbbq","nqtc","fv","jda","hb","ywnb","hwm","kfhg","wvhf","gxny","fmwk","mwcz","gxw","ztax","cby","fb","wyq","pzbb","vmt","bi","tb","jtmg","txc","cpw","bypk","dy","khq","yj","dqb","mgb","xa","zh","jf","afvq","tb","phbm","cw","yjwa","by","yj","qxdp","ty","qd","byxt","gmk","icn","yh","atyy","jy","pi","wgac","bhp","gvn","cwg","qngt","jvwk","nm","fjb","hft","fbbn","xytn","hmj","tcc","bwib","xyvw","vb","hj","wzf","fcmg","ty","kn","zy","chb","bhb","hjvx","hfb","bawy","pywt","bfbq","hw","kn","txcw","qb","yj","zyd","yawk","bq","fpnk","bk","qzd","qftb","bm","qny","dxv","bni","cmb","phfh","mhp","jp","yi","hgim","bpd","ck","hb","bq","bac","avfp","kb","yjw","xbq","btyi","hxb","jh","whqc","btc","nyja","qbcn","wzcb","yh","hih","nc","mth","ybpz","hid","njb","wtq","yjmh","bb","jt","dth","hnbc","ydpb","hib","nbjh","hnx","cajf","kbx","qxm","bh","xvbj","hw","hnj","xiq","jyq","vcc","cwpt","fd","gb","jqx","vz","wqz","ygh","fidw","pcdm","nbma","mp","wav","htbw","paz","xc","pxi","cd","gkw","fq","hk","hdy","wb","hxtg","th","khdw","dxv","vfqk","yw","mwcd","hnwx","qci","kw","wta","hcyy","cdn","my","wzj","bcgb","wi","za","byd","da","bb","wmkf","mxq","wp","xkt","fhvc","ciw","ykm","ka","bbdy","bc","bzxf","jbw","jfc","mbc","bh","cq","yby","nbzh","vc","fj","jb","iza","gx","icmc","xwn","tcfn","ngph","mqcy","ygqz","wnv","hbm","tymh","ghzp","ckf","jh","mxay","pbya","qnhd","bhw","cvzp","bg","igw","ykcz","hnpf","yhqc","cdig","tx","bbf","wcf","vw","bqp","hyt","xtb","nwja","fdbv","diay","ix","wdg","wbmt","itx","maxh","xcbg","yciw","icv","bfq","gjwf","kw","wfb","hqp","mxi","wy","vb","npx","hk","hxy","zbv","qybj","vw","wgxa","cg","pk","xa","zbkq","mt","dmw","dx","gt","fbx","ck","bwpx","ybwv","jx","bhtj","iaj","if","bkwf","wbg","wa","bcf","xj","gbz","tj","wyb","cb","wtmc","hhc","zm","ih","bcw","bc","bfp","nkc","cwc","vh","bcy","bynh","bh","bhpm","zchx","ibvx","kicc","ckbg","zkht","tchc","byq","acx","ctp","bznm","hhbw","jhgd","gb","mhwn","bcd","jct","gwvb","gbh","yh","xwkz","bzmw","zpt","bkwb","cp","jhpb","ybq","jy","xjhy","qaby","fga","cajm","xghk","bz","hgw","myq","dnxt","kqww","qg","fw","gczp","dgyw","wn","bkb","acmy","ihz","bgmh","yc","nhhq","mvta","fvbw","twqn","cxtw","ajw","wyqh","yavd","xbyt","bn","aqc","wbda","chk","ywaw","cb","wpz","kxz","agib","hd","nhwf","wya","bccq","wbi","dax","gc","cw","my","ccvy","wgkf","dcbb","cbmp","fbpm","kj","qwcy","mk","bvct","cch","dq","bwim","wdy","cxmk","bv","ca","bh","bwwz","wix","wq","nqwb","niyp","tjv","mdxp","hn","gy","fznb","ybz","fdz","tv","hbt","cm","fnjw","nbp","ack","vbw","cjci","cf","qy","vam","wht","ixm","fgcc","im","hbx","mbng","vtwh","hfza","piwf","pwh","ympb","bc","ibz","zkv","bka","pcch","yf","xqc","abzg","aymk","aby","hym","bk","whdq","bfy","pwyb","ch","gab","kvw","gx","hfgv","mbkh","wch","bgk","pwg","jf","xmw","ywc","tghw","pw","dnwa","hbav","nip","zmgw","dz","kcvg","bpx","mic","panb","iz","vfbj","ah","cpv","bidx","ibyh","bhn","hckt","ph","hw","bdz","fbbq","bpk","hdw","nt","bw","ztb","yxw","zp","ytj","qj","vyb","bti","aky","pqd","wxgq","bbkb","wqi","wbdg","wt","bbdy","gncw","bycm","gh","wyyt","ijc","bh","nax","bb","hqx","fgy","yxh","jhh","cfi","hpc","hgb","txb","kd","cbgw","vbn","gd","hvaq","yjtd","dbct","vkbh","bd","kbn","faqh","ibjz","ntib","gy","dq","qk","kv","zhvt","byj","gafc","hcv","mqba","gnd","jiq","wy","qf","hzbc","chym","nq","bht","bywb","da","hfw","cfqv","bxc","zhj","jyc","gcw","ypz","kbyn","bhg","mc","yb","czkd","hj","wc","tz","vihy","yfhp","vgh","kj","xp","ijb","cn","iw","jwic","abzb","chb","xk","zkx","ybh","tayq","yw","gca","jby","pb","bkc","bbyf","qi","nhyw","yh","mcy","hwkx","wygc","gx","wbxf","hq","jban","mdtj","mi","ag","xcd","wz","ybnw","hwy","ct","bfyz","xk","ckdj","pfkg","jfc","gwm","mbbg","fm","za","ahpm","yhjq","wnbv","bz","zk","cp","znf","pymh","qwh","jm","pyj","avhg","nmc","bhh","gz","qmz","jwc","bnt","ghby","zp","yb","dxby","yc","pgw","mxv","nj","qd","nmy","hq","nk","fb","mcfb","bi","whb","ydab","idp","adi","gbi","gw","hbvz","pbb","yc","wkgq","kxf","kbc","dx","by","kiw","zbtw","wfd","ytd","vimn","kxz","ktn","tw","ychc","hm","xz","zwy","dq","hkx","gw","nbjw","yib","ndp","hbfp","cy","wnc","gj","dh","yq","gwfj","fmqd","bn","yxnj","ciwy","cbc","ch","gt","dxw","hjm","wh","wh","kbph","fdq","hjhm","ydam","vxb","gykb","dhj","ax","ck","xhiq","gty","wyjn","abw","whfv","qjnf","xycd","ycm","fdk","mf","wy","mnxd","nhwb","nbbx","bmv","khjx","yb","kf","vw","iy","yjg","znyy","yzi","iwzh","bijc","bhf","yt","yv","thdz","ki","avy","bw","vt","mtz","yndb","ck","avdp","fb","ydfj","dmzy","tb","xb","wcnc","cb","gjtw","cc","dvn","wj","abti","wp","bbi","qzhv","yfh","bfy","cz","bp","zg","bwd","jnmb","th","bhy","bic","yi","fhmk","xzy","hz","mcqy","gh","ngj","hcfz","cb","ahvx","ajgv","jw","bmk","tb","hh","hcbc","nqw","bpib","dvjz","xqbv","bg","bfz","gzcb","nbzx","qtv","ny","byi","wb","cmg","hi","hbh","ahdw","imw","vyfh","pxhi","bh","ytfk","bdf","hznb","cnq","qhcv","vwkg","yfbw","hcd","hb","wy","bw","dym","tanj","ba","czwy","jh","pwqk","cq","yqfi","htw","fat","pdz","bcbx","cww","cgz","bbky","zck","bbb","qza","bw","yjyd","icf","hyhw","hv","pchi","ym","mwfc","jc","byac","jhdk","yz","wth","ykf","kx","cgti","aww","ydbh","mnzw","zk","cd","bxy","atm","cbx","pfxh","fm","qc","bdzk","bgca","tmbb","kbwb","tm","qhf","byq","gh","xbh","gd","zqc","jthh","yg","hxpc","ytji","xbbk","tn","vk","gm","xzt","xc","vfh","fphd","bwfk","vzxb","qc","by","hwx","vj","jxhg","dn","qd","htm","bdv","jb","ijbx","mvz","qbch","ymw","ziy","kja","jxbw","banf","vb","byf","kb","dbk","ib","qm","zbn","vty","nmvk","by","nwhg","gm","jyd","bz","wkt","fg","xbv","pytw","acbb","cbvi","cy","ha","ikxh","cbw","hin","ckb","hy","pycb","bbwz","ghh","xcq","dn","whnh","kmy","bdw","cxy","qyip","wz","tbc","ytmn","dm","cnp","wi","gc","pcb","xhia","bjc","myvh","tn","bm","dzm","nc","bpky","yfgm","bgcy","fam","jh","wny","hhmj","bigw","fa","nqgy","ph","ix","bc","zh","mi","zdb","ydh","xw","ymf","mj","cbw","cakj","ynwc","tn","xk","atwv","zabi","hmq","ty","nbcb","yhba","wqj","nag","mxhk","ptj","cbfv","vcb","thvy","ncb","tkj","gcz","ay","hcv","hk","hd","tgqy","vth","bm","gwc","bvjy","wcg","fqk","nag","hy","yd","cznp","vhpw","ymq","wbi","pdch","xzy","ycf","iw","idma","dkij","nqbc","jhzp","hcma","wc","ig","bhn","wc","wywa","atpj","jf","fc","ctp","wcf","mn","qc","wkp","ckft","zcyn","yxp","hbv","hxfk","hwpm","fbhv","nbyb","wacx","bh","ij","cgyt","kb","xc","jax","bbn","cqt","hwkm","tyb","qbxw","bzc","hbk","af","tmb","nj","bn","ywiy","ht","wzb","hc","ybhm","bhx","cmit","wk","itfc","jpd","qbyd","zb","bby","ab","jz","kvxw","kq","in","nbq","bphv","xb","byb","kci","jd","kpq","vbw","ah","ybpw","yww","ba","mbf","qwy","afj","cmby","byaf","jzc","jp","znth","gm","cd","xchy","tcc","pib","xygt","cg","nf","qbbh","kwz","achb","byx","mf","kvhd","dmk","bdp","yc","pwi","jb","yk","cd","mbc","xt","ygi","hhc","bmtg","ynbg","yxi","fpw","bk","qy","ijqp","ig","gbdv","kmby","wc","bzf","acfi","hq","phww","dycz","wbqz","cw","whjd","hf","gw","jv","ymqb","hybc","tbcq","nd","bx","hw","hq","pvf","yhx","kha","cz","qhw","jax","dvch","czh","yaz","my","zcfy","wft","nv","bk","izh","bmx","mt","bjc","aq","qw","xn","dch","tp","wnc","cbq","kjb","hcyi","cp","cy","bfy","cv","hbf","tz","kyg","wh","bfqy","zcc","iyz","yc","hikp","xb","qhzb","fd","cbz","ky","jn","ycf","na","cfjw","xmk","htvw","zp","jdwa","bgy","awwx","wy","wh","wzmn","ybzn","yd","ga","ky","ybpn","acth","bnj","nb","jwy","pvhc","ja","qfb","qtf","xmd","jcw","qp","qwiw","bj","acgz","cw","cy","qfcm","xqf","jc","fy","xyby","wzq","btwc","bny","idw","bab","bay","bh","ty","fh","aw","byp","pchz","bt","hm","xqzc","wbdc","mbb","gjtn","ww","bb","cxd","wbdf","wz","gyc","dbbj","wx","gbm","yzb","kmbc","ybxp","hi","fbp","iv","qy","wc","yy","zhb","cfhx","hj","jkwh","vg","wyk","hk","yxi","kjhm","izm","zwha","ihcp","hkgc","hab","bb","jydp","jyp","bmn","biwn","hk","xqbp","wb","npg","mwpb","ibz","qfcb","ywfq","xih","wynh","fyc","cit","ycb","cg","wct","wjyc","jm","wkh","qtgc","zk","qxgp","mwf","za","xmqt","ivyt","pb","pdcb","zdf","kmbf","va","zv","yw","vh","cwj","vb","qxy","jzc","iy","iy","bch","fkyg","tg","fz","hwcf","cam","xvd","ga","hcg","hw","yw","btcd","bw","pm","dgci","yg","wyk","qmkh","xq","gk","gi","yfzw","bt","wth","wcy","gbn","nwv","cngw","inp","cd","fjpt","hbty","yc","ch","yvph","wf","hj","ihya","kgcq","dg","yik","yzdx","wy","axk","bw","wx","wg","tbx","jfwm","pfj","tg","izfm","tcw","wi","hawq","kv","pgxw","yq","wb","gan","ty","zhyg","yzcb","tc","bcgf","pwb","fdx","wz","hgqz","cq","ty","fzmk","bw","wnx","qbyb","tnph","hd","ij","dgm","mkh","xb","xhk","gbc","ygwh","cyy","bth","aqb","pmh","gbc","hm","ntz","tj","za","fc","zwy","xw","yh","bhh","bba","hbv","fa","qc","mhx","in","biq","fmw","vhb","pkjh","tbx","ma","vha","dbx","jyvd","pcv","vc","bhch","dx","vnwz","dbb","mtd","hpqd","btbw","np","pva","bm","xcdy","ngx","baxt","wqb","hbfc","zm","gyb","hvq","mgwv","nmz","zygd","cf","hyby","cz","hz","hdw","zwnm","jywc","ywyc","bwhw","hpbx","wyq","mdhy","yybb","bw","qtf","yv","mbdi","fhn","adcy","gwpt","hxtw","yw","qbxj","bna","cxp","bk","xhz","pcz","yjhb","bzip","qm","tv","byv","ga","vpda","gyz","wkg","kid","wxa","ikdb","pqmf","yfg","gbqw","vb","xy","db","bhn","ytww","bn","ybbz","bqbx","hnc","bvyp","kbyv","pjn","di","ph","ynxb","qbd","hwjv","qb","wdny","hgxv","fi","bcw","aw","yc","gh","hcx","yxbh","mg","vbt","bgkc","jd","hwga","ndh","bz","wcv","vhcc","kw","vh","wvyz","gp","wh","zx","tn","wqf","cjgw","kbt","py","zc","mip","wftq","byx","ciy","hwfi","wqmv","cch","gtv","gyjy","vtqi","yp","cb","kx","wb","ha","ckyb","mccn","hp","vygh","thpk","mac","bfx","wb","wwv","cphn","pky","wbf","bmcz","yxf","tmf","fbc","ybj","micc","bnxf","wz","mfgq","ab","hy","hb","gb","hyib","bhc","bh","ikcz","vbbx","qy","vma","acdn","zwqy","yv","bdz","tgw","kv","cygb","wbp","bmxh","dc","fymp","wn","mpb","dvcb","fyn","ixm","hnz","wmp","bzy","bbw","pnhb","mbjb","mwxh","tf","tbk","byi","bif","jnhi","mnpa","htk","gmbv","igch","ya","mhch","fpb","yych","bv","ncgx","yncm","dka","jhky","chz","bf","zdwc","qwic","pibc","yy","cxc","cgby","nwyc","ft","cpmd","bqdi","qb","fy","ywi","avmh","qhy","tbbw","tx","qybd","fbgc","dg","bcwf","qbh","pcy","pfv","tvi","qk","hz","wpa","wd","jfdt","fa","ha","yvi","yxg","nhgj","ict","atwb","qmi","cmbi","jma","hmhf","tb","zh","yc","qi","wwc","vd","xvpi","fz","bn","ythw","bay","zfd","gwhf","yb","ayb","qdba","pix","yc","whx","fcg","tqab","iybk","hw","cn","knc","htyy","dhc","jybg","nwjz","zbj","fjvn","adbm","fxny","ndb","bxb","vwpz","jva","hm","kqwn","mdbw","cwy","btp","hqif","wbnm","mzk","ayvg","hq","njx","txw","jbh","fnpx","wth","zbkb","yzh","hv","gky","ccak","wy","cvt","phfz","yjxq","hy","id","bbhy","vq","hf","ibby","pcqy","wj","bf","mhyh","cacn","wt","iy","mpiz","ntap","ahq","zqi","wyb","cq","bbtw","hy","bj","zh","cqdj","nt","yiqb","acxj","mt","vjix","byfa","nhd","tnzd","hwcb","pdm","ihvd","wd","xwdb","fhy","ix","ct","nwkh","pyt","xdk","ydcq","hxhb","bqwb","tcg","hy","khi","bixy","fyqb","wv","hpbx","qhbd","hqbt","ny","cnvy","cqbd","tcwi","pfwy","qpg","gcxt","bc","ygy","wg","wjf","ngvz","ydi","avhm","qy","qxt","axni","kyfq","mwb","dh","gh","zyww","hbjm","kz","ay","fqy","dh","bjf","az","py","qmxw","vb","ykw","fbw","nv","zt","vn","yjx","haid","cbh","gc","ybx","btbx","by","izgj","mbca","ky","pmbh","fdv","hp","wbav","pz","bw","vakc","jx","qm","yd","kcmy","byby","wb","gb","vxm","qk","vbwb","zhhx","fyt","yvwk","pz","ytv","iv","bxzd","cpdb","qyg","byjw","zxw","hmjh","fyp","mxcz","hhb","xip","tnyb","ca","hwvd","kwg","bjv","ybbv","ptv","gy","gxd","px","fbnc","yq","qm","fzb","xwk","qhb","hh","fjpw","wx","qmcb","yw","fc","zc","ixpb","ncwx","yq","xm","cf","vyyh","ncb","ycbd","zg","pb","gyhp","xmb","yn","cz","vic","btc","ix","gd","fcb","bbx","gw","waih","kiaj","kmgv","bf","cydw","gxpc","hj","bby","jbzh","bnvf","zcw","nbhp","txvz","bc","tbzf","bw","jb","ga","djwa","hqxd","gfnw","zy","tq","jdvh","vb","ky","bhti","hqb","ym","qp","gbvf","wkg","zw","fgqz","hiqg","im","zdmp","dyq","fjc","mwyj","zkg","xb","gvbd","fb","px","nb","gfmk","ygyw","yf","nxfc","yq","qg","pk","ywxb","qk","nv","phfc","gj","tc","pwc","fk","dn","hi","cb","gbb","kyc","cnhp","ch","py","awmp","fb","piw","aq","bv","dthx","mwx","yxbb","tqac","cnb","ta","cbm","adk","hwnz","mh","qi","gvt","xkb","htm","cy","zbbv","pc","ibn","nqi","yqy","vag","bmk","qdhc","yv","tz","wjzb","zgw","ib","kip","jhc","hfj","gaz","ynhd","aivw","zx","ww","px","bgch","bwp","jnq","gzpa","ynk","fybq","hy","gdy","dp","jxh","bf","qcm","wc","zb","fc","vhdp","dinp","wm","bz","gvz","ik","chzv","hmi","gjpi","ybfw","manj","dw","tyd","xww","tfq","yya","ywtb","ikbb","hh","wq","jw","yj","vbbc","xmbk","dbhv","yb","ib","pm","bfh","bkv","bx","imbc","jt","bdf","bw","nmqv","hf","xcw","tiay","yb","njtq","kczy","ika","vfh","wid","wa","bzy","hb","pyb","dbci","wyzq","ctpn","zqy","tgn","mxc","whhn","bghv","db","btd","ki","nmxp","hc","tw","fxqh","wc","wcy","tb","yw","gvk","ywqg","bz","bbch","vcm","fbhw","bbzw","gcfw","mh","wg","by","mtih","ity","dh","yx","wcb","txh","dbqx","bnhc","vhb","anqp","dq","fq","kh","bxnb","kfy","cijh","zc","xhba","pg","hcmj","xcj","bhxa","gbv","yg","mb","ztvd","kyh","ft","hqb","fwvh","kc","qi","ah","zh","gbyi","hip","wy","jzw","jat","hy","zcmy","mnw","hbp","xm","ibdn","vdb","ihjc","wmfg","ya","bv","iqc","whc","ky","tzw","mbb","igd","yhcw","mpc","twk","dz","jh","gbc","ty","wx","dbv","hgj","cyb","xt","wknf","ci","cd","tca","tyc","ni","ptq","cyp","dbqp","bwz","bb","mtz","kyqa","wy","fcj","wdkz","pc","qwy","ybwj","hpxb","dj","by","ww","nvb","wxyw","vzt","qybw","wkf","zj","mt","iwhy","xgw","tyw","fmcd","qb","tnx","fzbn","ckbb","dwkh","phv","af","by","qpw","kq","pc","jwhb","fd","qi","ip","kcd","bgnf","pgbm","wmh","dbi","yba","nq","cn","hmb","ga","hah","jy","pmi","qkma","ach","cwd","wqmg","tn","gwc","ay","qaph","vky","mw","ayb","xwm","tjh","jdbi","pwwt","btf","wmty","gnfq","zfc","nix","mhb","jmwq","pywy","ifh","wbfw","qtyi","ybf","pmcn","myw","ab","hf","vxmc","bcyn","wa","ic","kzqc","bci","fhc","dy","fy","fyd","bg","zhjb","wybh","jmgb","bx","pxi","aqc","wbf","cyfp","qh","nv","acqv","pbkn","fyv","xt","fcn","yfgw","bz","dp","wb","wpy","mchc","gaj","jahh","mfhz","zfi","qg","py","hgxz","mxy","fcig","pchh","ynmz","bp","ydf","hka","bx","bfb","tb","pkb","ygwb","pnwf","ny","gyp","bwbm","fnqj","hbcg","fa","bwx","yi","khhv","wcb","bt","ig","hzb","cqk","xybw","zxcy","fj","pkgw","wgi","wwdc","mx","kd","xit","bbwb","hm","vbw","bfk","wa","xkbi","bqc","cd","my","wpb","fm","fc","yhxg","jg","gw","zbd","zdbh","yh","xcbj","wax","ncb","xph","wcyb","qbgi","kym","pf","zk","bncc","bqhk","pcf","cb","zbiw","cwax","xtna","iyw","gjy","af","mgt","cj","xfj","mn","ymgw","gb","cngf","abqm","yn","cf","hzx","yncx","xqvi","ycm","zpyh","ygf","chbz","dbbg","cqk","bwv","df","cz","np","wdn","mwn","zyy","kww","dp","ywq","xpn","dgnt","hy","yzmd","jyha","bbnw","ihzm","avh","qa","xb","bbwh","av","bc","bi","qvh","tdn","icyz","bdh","xqzf","ybxy","aqb","xpty","nb","hbbc","cyn","jygd","gib","wyi","inqy","gqdc","gh","tx","wp","dtc","pw","mzb","ctb","apq","bthn","zdyg","zp","cx","bw","ac","wn","ibx","xzb","ib","qbhf","phk","fa","bwa","hgcw","wg","ac","tpnh","abpg","vbh","zfv","ngh","aqy","wdan","yq","wgh","ka","pz","nv","ipk","advw","mnid","yh","cbd","ig","cww","cw","pfhd","fxkh","nx","twyc","ybmq","cxn","kiqy","wb","vt","ha","cpjf","kdz","td","ibxt","nyb","bxny","bhnb","jbw","yh","zcb","gdy","hihx","ift","db","qwt","fmdb","mb","ty","ctwx","ym","jd","ymjd","myb","an","vc","tcb","wafb","yb","fdb","jm","hcwk","hm","wcdn","mhz","fmvx","wqkg","ichy","jkv","hpd","yif","cnxh","px","wq","ycic","mnw","bzj","xkya","wbhi","chn","txyb","tchk","kzc","cwtb","bq","ib","yik","cc","hkb","wy","znw","xp","xnb","pic","wabc","gc","df","ncbh","cqj","wp","yv","jbqk","jvpb","fih","dba","qv","wccj","wavw","qcc","gzv","fv","yhtb","fcby","xckc","nbb","cthn","qvfx","gm","bh","aw","bj","zhw","thfb","bjc","hm","ccqy","iw","vpm","jqib","cbzq","zda","hy","xcnb","wb","qwg","jbtc","bdm","bq","kcb","ch","qb","py","yik","fckn","cc","yvhp","bw","ijmp","ahbd","gfz","fk","jb","cvx","awc","qyg","hvy","hph","zi","bvp","aif","wb","zgf","hybb","fhnd","km","xjqm","wyh","dmpt","xh","kypn","hbtx","wn","bzj","dvpb","chy","bphj","djc","bq","km","bx","wqbm","yzhy","bxh","kj","bc","aiyn","xz","yni","bbtv","tim","vnh","ivjb","by","zxcc","ihb","gcb","wb","xhq","tgc","hbt","yq","mjbf","hpfn","bywd","bk","wp","kdzt","iv","fjkb","kzp","yp","yi","thk","qfa","kw","cf","cik","bym","hhm","wcqz","bqhv","ybcn","tmj","th","fcg","hv","kgdy","vn","tbyw","yhgx","dyfb","mwk","pwac","nfip","tiyf","tvx","ikwx","bcvw","kfbv","ymy","yjwb","dhm","jy","bgzk","tyk","ht","hbgy","hwb","tb","agfb","ywfg","zb","qd","hc","wc","ym","zbqy","dnp","hcdf","davy","bhw","qaiy","aix","higf","iwfk","bhjw","ibbp","tvz","whh","gkpw","nhp","bc","yiyk","cfyi","yn","xyb","dnwp","wk","th","bwb","jmn","jbnb","bzkw","nh","ch","cf","nqy","hi","vx","iwfb","vbba","pkjc","bkb","pa","kvbd","hn","ji","mtp","wym","cavm","fbiq","iq","tqmb","khaq","cw","kzb","xb","hjw","bm","kynb","tagc","twgk","xfn","yfa","bw","bm","pwt","faw","cj","hmb","bgdw","ya","yqy","bbm","gfhy","dhwk","xcjp","nwaf","hdbb","ifyb","iv","xh","mg","md","itkf","kw","wbc","nhp","cbn","dwqb","gfc","cvbh","mwc","ckmv","kcx","cth","viqb","pnmy","bic","zw","fy","njv","vy","hihc","bj","ba","jqz","hfp","hwy","bg","dctw","qhwn","tc","gb","bcd","hyc","ibcw","xb","tc","pqbf","nb","yfic","bt","fcjt","diw","zqy","bbyz","xnd","djtb","wc","bcfa","fc","cp","giy","dqbc","cz","cbmy","hhzc","wkyb","ynav","iak","aht","atwm","jzw","vd","bmh","ifh","ji","cgyw","chyb","bc","fbh","qd","wpm","hicj","fkg","yhqz","bq","ybx","jhh","yt","qfxm","matz","hw","nc","yinc","cwxb","czkf","hi","kbw","wpz","ch","hcn","xb","kgi","bj","yn","hjt","mcty","aky","nxh","bijw","piw","kn","yqc","zb","whiv","yi","cbkw","xb","ykh","hcq","pykg","hbc","jbtz","pn","gkn","iht","xw","thp","tb","wxca","ia","mw","yz","xnc","ak","zq","pbvi","dmcg","fj","gxb","ywnh","caq","bhz","bjdq","bbda","vx","idyb","wqh","qbbm","hbm","hdf","ab","bgky","ccp","hdx","yngq","fj","wy","ym","fijp","bbz","ywv","ync","kw","pd","bbyn","bfk","qh","xc","fwh","fiv","gjmf","icc","cwqa","hdj","wc","mxwv","qvy","ag","dwq","zf","kby","qi","cyzw","bmcb","kbyb","kwcb","bhky","vkx","tk","wad","cybi","ybn","ct","hb","zjic","vpq","jd","ah","hjiq","mhw","jyt","mx","ad","dzxb","ckyh","zhp","jc","qj","iwb","wqmf","ckhz","pyw","ak","avyw","pkxg","yb","dwz","zbp","jydh","kbc","cg","azc","bmbc","kqj","cmjb","zxt","zp","xi","iq","dv","yi","jqcm","qj","wpjh","if","ybh","nhk","pw","xmd","xg","kx","bx","hna","bph","zhaq","dqyy","kbab","vmj","cvb","hpzy","qpbw","qb","xq","wdhh","bnxg","jnh","bza","gzn","ym","knb","wb","cwvb","cjy","yz","cxhf","nf","yhb","nwg","byb","bft","bpth","wvn","zfb","cwpb","jcbw","cb","tbf","nzb","nay","vt","cz","qndt","xva","fc","kb","wa","bcwx","wm","ykht","jhb","mzn","bv","bf","hwyz","hy","xv","whhy","ijxc","hwv","vha","bchw","bjtc","wgxd","iyb","vg","cbpb","bgph","chw","cgc","phw","nkwy","wqt","fbd","bc","bbc","hfd","higw","bfy","mh","ycgx","wcmn","bcb","bai","wk","dw","xhy","nwgh","mwab","cfbk","jp","fbwd","qywx","tbb","cv","izax","btp","bxw","cx","ka","ab","nj","zkc","njw","fqp","wb","bm","zt","cw","cbpk","yi","tkyb","twck","hpz","pj","dw","hcf","hy","giwy","yf","ghb","pmfh","vyb","wh","hcp","bz","xbam","kapm","dcfb","pw","vcat","pzbx","dc","wnx","gy","dgmc","cy","hx","awdh","fyp","dji","wdp","ytb","cbxc","nkx","wc","fgkd","wkj","hba","pcbh","vkn","fdm","yhm","ygbz","afnj","bwt","ciw","mbh","tnz","gkj","tk","hy","yyba","ywmx","fcwy","hch","apb","vxby","bix","viyw","vzw","fva","ak","nhmk","ynf","qfbi","akdi","jgtk","ywh","dn","dby","cd","zka","pnct","hnj","in","yzbb","xjp","bzc","qyn","gt","icyk","xfz","nyya","kw","whci","bhmn","hicz","iqg","bxic","tg","vip","xzd","ga","qihw","bd","xk","yhhz","cbn","apf","ag","jn","bhcf","gp","mczy","nyf","kc","yw","bb","wt","awz","iyc","jfi","pbc","dk","yyfk","ni","xh","mhz","bk","idg","wd","cqbj","nzcf","dgwc","bv","cwqf","vh","bx","jpq","hv","qhjb","ibc","pv","zwdv","xa","ym","bh","dyc","nm","hb","ayd","ynzb","pgcw","cvbd","ncch","fp","dbw","bwcn","wk","hbgq","zhf","hyf","jawg","cvb","yx","qh","gdx","jbh","qca","td","xz","twk","vb","yb","vk","bcy","ikbt","invh","viwh","cxb","hg","ahp","wv","dwc","awy","bwj","ywpn","fx","bg","hm","ahg","bj","wcbh","nfwb","cph","mn","bwy","bph","xt","qf","hzm","pkx","zn","qck","fvqm","ychw","nmh","nt","ytm","bdta","xh","jmhc","wc","zx","pv","gik","gq","miyx","wf","fqim","jx","xbj","wvh","nvm","bky","fnc","ct","bx","kjw","zywj","dhbi","pm","ypg","ay","wfb","wpy","anxt","hn","whfy","tdcq","thnx","bmhh","kd","mbtc","cma","cnb","dwm","wdbz","pbvn","jv","qhp","kqd","id","ybw","jk","dzw","cd","jvqw","zhvy","nwp","ytkw","cc","pykb","qzy","hy","hknm","ccw","pmy","zg","kb","akc","bqy","vj","gmyz","awbp","azq","ca","jcct","vyw","qh","hx","mytc","qvw","mxwj","mn","bmf","jwh","jchb","mgv","tnz","cba","mccx","ycn","fh","hjy","ab","wghq","bj","afb","fwhg","vwck","mf","wp","twhk","vxb","bt","vh","bwm","by","hpyx","jvky","jqp","qwb","hmj","fb","qjy","yah","kba","af","wp","pkwm","bi","mt","iqp","ph","fj","cigc","mpjq","pmn","qczn","jpi","fkhi","dbik","chf","cqx","pxdk","hcqf","jt","wx","amdk","bhzw","zfg","xjpw","vi","nkb","xy","ijwh","vzwh","zw","ybwp","tyw","fdzb","ay","tawh","cb","waw","qzc","wtk","kwc","mcxb","gfx","icf","jwh","dbbp","txhi","hg","pxb","my","zt","gd","ybhd","qibw","xy","cb","bch","yb","xd","ch","yqw","yp","ym","tn","hbn","bg","xjig","aywq","pxb","zw","kx","cykh","wci","twgc","gdnc","ih","hh","bf","ichb","hcny","wb","btbx","dx","kiyw","mx","bxp","chb","tc","pm","mw","vhb","xik","bhb","hah","xivt","tc","gqx","wy","tfhn","jqhi","dymw","jp","dn","fkam","mvd","iwy","cbdw","hd","pw","vcat","vtxg","mwc","jcnh","mbiw","jc","ycmg","wck","pvfy","xayw","za","nhb","yd","vfc","wncb","zpk","fb","ki","pgnf","mqch","wx","hx","bbk","cix","mzy","hbc","amb","bk","nxb","di","jz","fh","ytgz","mx","dyt","fnp","qw","jipf","vybh","fhi","ywjh","twgh","by","mc","wdnb","bc","fdh","ctf","qwcj","gbc","nwb","ka","tbw","yqvi","pw","dbt","av","xc","zdc","mfx","mckz","kqmg","yht","cpwn","tx","ji","bv","gq","mpfy","bwcz","nb","yn","gbkc","jf","fyjn","xb","cb","yt","awfw","mwgb","jw","wpih","gx","bwnq","bxdg","qvxn","yf","vkh","inh","gnvq","cyb","yb","htp","nh","avq","abty","cma","qwfy","ab","ybpk","pzxy","hqg","jnxw","kqwt","bxy","cqi","kva","ivj","wij","bzj","jy","xy","bwab","bpb","bq","yt","anj","wj","yn","ci","bni","hy","chx","cw","ya","gbqh","xgm","ig","fqiz","nip","ictj","dpq","wf","dyi","wyqk","pm","wjg","bycq","bfcp","yw","ayqf","nbgk","yp","zb","fdyb","ndxw","wcv","wb","hbgq","jy","chby","xgcy","mcjy","ngwq","bb","gbwd","dhb","dzb","qt","bfgy","kzx","hjb","wmj","ich","gyam","zvjt","af","gab","mc","wdv","wb","iwkm","ak","hb","pmq","tam","zwwf","jhg","bnw","yxc","qhbz","jxcw","btw","bhwc","hzp","cbg","wib","hbcb","xih","xgfy","yjic","wny","cyb","fn","cqfa","jcxb","bjwd","bj","gib","gvq","bnam","yjq","aq","zxg","bm","nafq","afhj","hh","tcxc","jtf","wzp","cfj","bjac","by","nb","fw","yq","ikwj","xbb","ky","xz","fb","dnyg","fwb","kp","ij","bydp","gbw","ai","bywf","tjbm","kj","wyj","cw","px","yb","hyh","bqt","wkc","qcjw","kn","bgk","ba","qcpz","bnc","xh","qm","tpx","za","wx","ccj","wqy","wy","an","bwf","cb","hn","wtb","vfa","qv","cyy","pccq","zqcp","qb","bxj","hhy","qwid","bk","pzcy","ncbc","hzc","khz","ckhc","kyib","jgyc","kjcn","bmz","bkxc","cjxw","in","fc","qhgk","zjn","gw","ww","yw","jpd","bwa","bgpj","byh","zh","bix","wvg","hxbk","bhv","jtd","zav","bbya","dyzb","zxt","wca","mbic","tg","qkab","gcix","ynw","bza","jbna","wbt","cabk","cbad","mwpk","bmjy","yag","mfq","cmx","tc","ity","gw","mc","fk","pcf","cz","qy","hbxw","dhtb","wk","igd","bbgk","hjwc","zhpj","typ","pqy","kwcj","bf","ixg","yvw","wbx","cww","cn","hwwy","pyhv","gz","ntfb","qmc","bzc","ifx","zy","in","fhyb","kv","bw","nkyb","cp","nqap","hm","fv","qcvz","gdwb","cntz","ptb","aymf","yn","bg","zx","qyw","dytc","yjbc","mcn","vk","wki","whh","xb","xwfj","wt","ib","bx","cfz","cbyb","ibm","nhb","wc","pbx","anhc","wpx","fphc","nbdk","cqhb","fpyc","fzy","bc","gc","zw","kzh","dwy","cctw","vwnb","cwnd","yq","nkyq","hkxw","jx","gc","tfwx","cdb","gk","mg","dc","ctix","jbk","vm","km","vkb","qbzh","jxw","afxg","bvc","fk","fw","pnc","yj","ny","yzdq","cwbm","wc","zqtj","dx","yvdn","iftm","pgb","tz","mwpv","gywm","zhw","apyb","babz","nf","kzd","cy","hh","ww","cm","mgk","bj","wbwp","wcg","zyyh","qtdx","byc","hvb","xw","jbah","ah","mbcw","jpc","hh","hcit","vt","vtb","htb","hj","zpc","dh","cmbj","yhxk","wibv","nc","ck","mph","tc","wnb","hwvg","cww","ja","gbq","phb","akm","iab","cdzt","cvb","tw","tywm","kzh","nb","gyc","ahp","mbq","bxcb","qkcv","bym","ig","xhcb","cm","bkwc","py","hqwb","xnb","kzcp","kyci","chb","bpf","ybj","hzgy","gd","wtqn","ccb","zfdx","tbhx","vwb","hmb","chw","wx","gycc","bfhv","pb","zcb","wkbc","gcw","mnyp","bm","xtn","bngh","gzia","bfhp","zn","jyzn","tiy","ah","zb","ib","hbm","znk","fqa","hjca","gqa","nvx","mpx","btx","hzhj","xhw","chh","nbh","hdg","wg","wn","gq","hbp","hjxb","zc","hfv","qza","vgh","bk","ktdz","bmig","cpwa","cah","wgyf","vxfh","yi","wz","itad","xwfh","dzki","twq","qig","pvx","hbww","kfny","nwxv","dq","zay","yadw","vcqb","gt","jt","cwn","fch","cby","hyv","hmi","jy","ycy","kfyi","cyv","gw","gpc","bj","bwh","ptx","knhx","fbhn","nj","ipnt","yw","dtcn","bp","yv","yybf","cbnp","gjq","nqcb","pqv","fc","kwtp","qy","dab","yh","qjwy","ht","ygaz","pvcm","yn","abp","icxn","cpw","qm","mxnh","npyh","pc","tq","bq","dkiq","yi","pxhh","vwz","txfq","ndhy","zgt","wi","bzx","kn","wmnb","xdhj","qibj","hfb","af","yx","wh","znb","qj","wcg","ia","ywkb","gyqv","cqay","wbhc","npv","ka","fct","agvj","in","wgbb","dw","cw","iyd","bj","gzpc","cdyi","bm","nf","fkbj","km","djfy","fy","mw","jnq","mxhi","qvcb","tc","fq","wify","xit","fcby","ywx","jhc","ybwy","bkq","pfcm","zg","bkv","fq","fbqh","bb","mhcf","gqi","twb","dvta","vy","mbb","bwc","zcga","xchq","jw","xzyd","xwfy","fwj","vzdb","bcpc","bdyz","yt","tcp","qzbc","cip","fa","hypw","txwg","baqc","qxv","baz","by","hizt","ca","dh","yb","hwv","ig","yt","zdn","xmzj","gdzn","gv","yi","ph","qhw","wbbm","yhfc","dh","xf","pq","wt","jncx","jwz","jv","ag","hnwj","cfic","qdi","hzvt","jagx","bqb","mwxb","bz","bm","cyhc","mv","hpbz","iat","mjw","chyz","zd","zxw","xvqt","ykhv","zw","xwh","cntb","hvwy","hh","pf","cwfz","ykd","azqv","nhgc","tx","wyw","pxhd","itwd","gwkv","qc","pvww","yp","wm","hh","bwif","bqnh","wtya","pig","ghc","db","jh","dvz","hwi","bgnb","yc","hc","tkp","tzg","badq","pb","miw","cmgv","nap","bcah","mb","pcba","fhwy","hwax","mdw","bhb","abcw","qh","gyac","iy","nzw","bbdy","icc","qkcy","ypvb","wpxi","ydj","yw","kbfb","bywn","txq","qhg","hxq","nhb","ct","bz","tdwz","fjgk","yzm","qbn","iykn","nzw","cmzf","df","ycx","qk","hy","qmah","bjyi","tdw","cb","jh","hpb","adfn","bjbk","dva","fgvh","vygt","inw","gq","bwb","wdq","nxtp","dz","gw","ad","dtyi","zyn","ag","bgcj","fx","ycj","jfzg","pha","thm","ja","jcd","ht","qcww","ibny","qvc","by","ypm","wvh","fw","bacx","cnv","yn","yy","wjb","bnga","cdpi","xzj","fn","qngb","nmjy","bt","dam","wba","cx","ngbk","byp","mpjt","yhvk","bc","czbx","gm","ndap","kb","bh","wbcb","npbb","txay","hym","gft","vik","fbai","ihq","wav","bwg","jpm","zv","gjxw","xyi","bjqw","pg","pyx","bthm","bd","ibj","pihy","hi","tnkz","fhz","yb","zgn","yg","wjy","iv","chwf","bzb","whfa","xbhd","df","cf","bb","hndz","ngj","dzcc","gtb","fydn","kc","dt","ty","wt","hc","ikbh","mkyg","ina","jt","dajk","mi","ni","qhz","kcby","bd","yi","hb","ixhm","td","qhn","ac","dcyp","hvjd","gy","jqx","hfj","fchq","bj","ja","bfn","nbp","dxy","dviw","wj","ajm","cx","hhgc","hpy","bct","bq","hxb","hby","bhq","zahh","mw","dh","ktd","gh","jh","jcpv","ckhm","cv","bcwj","wg","gkf","wazk","cbf","mybb","ybqz","bj","wpkg","bi","ygq","cc","nwb","jhn","qwhi","pv","nk","gt","kfhd","hpkb","hyca","ybd","qc","wb","xpc","wi","awdb","gy","thn","bz","hz","ca","vbf","jtic","wqzw","aqf","ghtc","bfp","ykv","gwaq","xp","wk","yvq","qf","pcz","vf","hcb","bf","cat","zt","phci","bwfc","ytc","itwc","kqd","wbw","mq","hb","bdk","yhgj","btx","kt","wbm","mvg","vwh","yb","pdyt","bywf","vyf","bd","dcy","xndi","by","pdk","cgwh","vghq","hjn","gwz","vhcx","wb","mq","bbpc","wv","gt","pb","dp","czbg","py","kygn","bmha","hpfc","bbjh","bh","hmzb","pvg","fxgc","nch","ck","ih","tw","wmaq","bxf","wb","hca","vg","hyg","gh","tcf","fw","na","whp","vybd","bwt","bc","agvb","hcvb","dbv","ywfa","ibch","cayq","fhzp","hnwv","ykc","cwny","cpm","bwyg","mfzb","vwf","fb","ma","wnx","jpx","bwf","fymt","vwxg","wdc","yqfy","xihg","tzwg","dp","fhv","zy","hz","yfx","pdm","bacz","pvg","vc","wkb","dvk","htp","pw","qhv","cxmg","bvc","bzay","vmzw","aibz","nt","bmcd","bvhw","wm","gbbh","biad","bna","yaxw","wyw","fpvm","ww","gcbh","tn","pyhj","fh","pbj","hmw","kw","pf","zbn","bhx","cby","gb","pxjc","nhy","pkb","kbwy","bc","at","jb","ya","yh","hanc","xyn","ah","qw","ygid","ht","pbb","wqb","tbzg","myy","gib","gy","cyh","gib","xwmk","ji","qwt","kbfb","bb","wkah","aibm","ab","hi","kajz","wd","gip","bj","ckbh","niw","zi","wc","tcmk","anib","igt","ipdh","cmby","gqz","fxqt","bhhy","gm","bz","md","fxw","tdc","hfav","qbna","hy","dxbc","hwpb","gz","ghm","hvyw","ip","ftba","wq","py","dia","ik","dj","xv","pn","bgni","fcw","bwih","ybi","wnc","pb","xy","bkyc","bia","mxcg","bfbt","dw","jc","ihcy","fvi","kjt","hy","pi","imp","mkiq","cyaz","jn","pky","ayi","abgx","ct","dn","fw","ygyn","byf","tbzy","igcp","ka","ydbi","ytib","dc","fp","pjf","nb","bwwb","ytqp","jh","zk","bj","fp","byig","ywqb","wy","pcyi","yc","vzb","gq","cpth","jygi","wyxh","kivy","bjw","nvt","yayb","hpz","hgwp","hxc","xm","wqdn","fb","kb","itjn","pbkc","gmt","btd","kvz","hz","vpb","gvj","fwtc","pk","hkcq","vcb","wbb","hb","vc","yifh","bpk","vfjx","yf","dhf","vfyz","xhpb","bzb","cbyi","ak","bzcd","hkhz","ph","ha","fvbh","bvb","ifbz","yd","pmcb","yc","yw","hptd","wb","hg","wxb","bhw","yn","bq","icb","nhhy","ync","jymw","bgw","xg","yz","wz","bwcm","vwi","mab","bdbn","njd","mbb","kwa","iq","vg","hi","ztjb","dg","wbhf","dw","gfbb","mb","my","im","kcvh","cxv","fby","xbnc","zvp","yg","pay","vbdi","zq","bxim","hck","dg","wbz","kg","mp","ww","kq","bb","ci","ib","xh","haqn","dnh","tb","bhb","zia","by","yb","mt","apiw","fc","ktv","ygm","gqb","ij","bpca","nt","jhgm","zbyx","mytb","ybi","bz","bvxq","ntzj","cby","abp","mvjc","bwqg","ix","gbd","qkb","icm","kb","mt","qc","cqy","wqzh","xn","gt","an","xh","yjfb","hcy","ccz","hxwi","ypi","mz","yb","zcm","dwbp","gba","ycbn","fiz","qck","yj","yic","ihk","im","qybc","tza","hgb","cw","cmz","ycj","gzj","xyf","hfp","abcz","ifmx","nh","zc","gfw","kmwj","tyn","cbnc","cjt","fwci","hywn","ghv","wn","jvb","mb","pdbm","dt","ihbw","wcb","txp","yn","xc","jy","vxt","iwa","fyv","qv","kiqy","bmg","vd","bnbj","wvh","nvxw","dbw","zp","cgw","bxy","bb","jhb","pgcy","yt","wcwg","ywbk","yzq","jh","qgfb","qyb","byfh","ah","qjyi","gxny","hxd","pwb","kg","cwkd","xw","wz","yhbh","gb","cf","xwqt","wc","dw","gqhh","npb","db","tzhv","cqt","vyd","yn","bi","zbpq","iabc","dbbv","bzwp","gpk","tc","wa","bzm","at","hnib","kiny","hxz","bj","mp","bqay","zim","hyw","wwym","fjyg","ax","cbv","fc","qbc","dwgm","xcmz","fb","yxfv","kb","yxz","whzt","bcy","dyk","wc","wap","nw","btc","tzb","qc","vnfz","zamw","kmyz","czb","piwh","xyp","xti","hbm","ah","yc","nq","fym","za","bihp","adc","db","xvjc","dkqb","ibf","hw","bmbd","acq","yb","yfzk","wgf","avw","yc","cxz","vt","ykb","qxh","jawc","wdzj","bdh","bqp","khmb","bwwc","hnj","mfkz","yw","jw","nyaz","bh","bba","icjh","xdp","tn","hfw","bhmf","kh","dbbm","xgbb","kac","zf","gcqk","kf","by","xhhw","xmiw","whw","abp","xvj","aczy","nm","dcm","mb","vb","gxit","gw","ibh","fh","wvqh","nyfw","vmpi","hkgy","wt","nw","zwaf","ahwb","bqyp","jghm","hxzm","hwdc","gjyn","tqyp","bj","zyik","cm","jfp","bhxh","pavc","mcb","zw","yjmk","bfx","gfbx","xwza","twkj","awd","bahd","xbqh","fwa","gzw","wbam","bt","bxdk","bhn","xwbn","bz","njpb","wgc","dpf","fz","hy","pwy","wwth","cq","jibh","wab","xb","zy","dnxc","cab","ycc","ythc","hwcw","vyfc","dvcm","naxq","fh","amwh","ih","tq","wn","diby","zyxc","fq","tc","bpbd","pkqg","jwyx","iyv","dq","gm","hg","vi","tcmh","mhfw","hpfx","qzjy","xbbv","cbaw","nf","hbz","nh","vgh","hyj","bxw","natk","cxwb","iy","mwb","qkxb","wfd","zjyc","fmyq","xavy","wy","qv","zh","hpnz","zd","ybnc","kwv","mhb","nv","bhx","cm","vkfj","hyik","by","pic","wxmd","ah","kdw","qhzc","vyi","wgf","cy","cg","cqbf","hxdf","wgwj","hwnt","cc","gbbq","ay","cd","gb","qz","gzm","hzc","qdhg","viby","pt","pvwi","iyb","yg","afy","hi","ybw","pb","jy","ch","biza","fivx","qdyk","zjyb","vp","whpt","yyn","cwat","jh","yb","gcz","mip","dh","vn","hnbv","xzm","pb","yc","nkxh","pcd","hfc","qc","fp","va","wk","wjdy","jq","gi","mcj","mt","wbyv","bbb","vzmb","bx","hyxc","xjv","qn","nb","cvwn","zqth","vbwj","dkqc","bbc","vj","yia","hd","ypxj","yt","wihy","gbhc","qc","dvq","wbc","kwbw","kiy","hhw","wkhb","jtc","chwi","tdj","bdtb","hany","bcqh","dpf","kyd","bzj","hhc","cdbg","czbk","gvc","yzcn","qgyh","wbg","hby","myyg","xf","wabm","yxja","bkj","tykn","xn","cb","xc","in","wc","pwwq","fap","mgb","jbb","nz","fbgq","nch","bz","qmcw","wh","ca","idcy","azn","tky","dj","gipz","mbvn","bc","cyh","wcng","iww","tc","bbw","dqv","zcb","fd","yxa","xm","tbv","zq","wcx","ijcw","bptw","wb","ib","admh","hb","ibw","cpcw","hbk","txzq","bnf","wd","hb","twg","pbd","ciby","nwc","ckdx","dh","yyb","bp","qt","ax","di","bjf","qcwf","dvw","ax","wtfx","jy","if","bp","caj","yf","qnhy","cw","fmk","ixj","hytk","vh","dzym","vy","xcta","wfw","yn","qy","jv","kg","xi","iz","zcdy","taf","znxd","bxc","widk","vpca","aqzw","bta","hp","ph","ccp","bcng","xby","wzk","bf","yb","ztdh","pf","wb","bx","vqc","zb","wv","xdbi","pzx","acv","kccy","kx","xtb","wgxh","qac","ka","df","zwt","zfv","xbc","zk","gw","yngq","dhw","byf","hyv","bnd","nyb","iw","zkt","jfb","wgyv","xhm","ybh","hhn","wx","vz","miw","kyxc","ikby","bqbn","bj","im","cat","am","igm","wqij","gv","bj","yw","axcp","hfy","tgq","kw","cgkf","tn","xmh","kvh","nj","kb","jbqw","fbiw","ca","pqit","tj","nq","nwq","twg","vbny","vy","wc","kiv","wihn","ydh","yyw","ca","yw","yyhc","cghy","qzw","khm","kzd","kvd","wk","hfmx","cab","yw","xvc","cwh","kcz","qp","fpj","taci","bnpk","kz","mc","wf","vfjb","ck","nkgv","bvz","wq","fv","yfyw","pqy","zfva","bx","hbk","bgv","jc","wb","bwpc","nbfd","hcn","gh","akbi","qwvp","khp","bb","bp","czjf","hd","dwj","vxg","iwg","fh","cyqm","btc","bjha","fh","wt","hfky","phc","qhb","th","ycga","qdm","xt","mvb","vg","yb","tdh","kbg","hv","awby","kdgj","fmw","hv","cwji","mgxk","hcmk","pcb","ki","ch","xfna","vch","gv","jiab","zhw","vph","nk","dmqy","jz","dcjb","hjyt","bfzt","vh","hpjd","vc","bk","jbz","tnb","cm","jap","hbyp","ah","wtn","tcni","btq","wyt","hbjc","bhz","ybnc","tybh","vcmy","hb","xyh","wkn","bnkq","tgc","zci","wca","bacj","gx","hj","ybc","bjmk","icxc","fxna","bd","az","fjq","wym","kbxm","cbm","bk","hwi","ywbk","iy","nhb","qa","jbd","iz","biv","xh","yx","th","xphv","mzhw","iwy","ipmg","wgyn","ha","wgxq","tfhn","fgab","ytkh","zdci","xi","kzgw","hdm","ky","awp","aq","dfw","nf","wz","whcn","nywi","xiw","mf","ya","pjwa","mpdc","pya","tgvh","tqai","hjzc","xw","tyqa","fba","cyb","tcch","tbc","myg","pa","znjd","fdk","dic","bjh","ig","mw","idca","yd","icb","whb","hwtb","wq","dzp","bht","wzq","ah","gcmk","xf","mpb","ji","bib","gnbt","adp","xwbp","hxw","ybgb","wjhb","nwzf","tyh","zmja","bpi","qtzw","wg","tfc","bvg","zp","igbj","gh","hxfy","thby","abc","cknz","bqh","cyxb","pbhz","qih","xy","ynh","bfc","vfmb","qy","dwcj","dpbc","qkbb","cwvx","bf","vm","hjft","bxd","df","bck","xyb","and","qfdp","kmcz","bibt","wgp","txz","bg","hyfh","haf","myxc","gnpj","wmd","hj","wmyp","gt","civm","yib","mwy","ibpt","ch","azy","chjd","cbmt","baz","dqv","hjzn","wzcq","bt","hvk","ypvw","nhc","fb","yxpb","cng","nq","dya","vmbc","tcc","wbd","yafb","hpm","dv","bw","pm","fyz","kwc","iwfb","bbk","hta","wg","yj","ydyw","wi","kg","fyb","hw","tz","aigk","bcb","zh","fxkb","hzfh","xgv","bbx","yp","cxf","aw","nzfa","vcp","npf","ybyt","fmj","bhwj","pc","vnk","cxm","wzxt","cqb","jy","kfb","iyxb","xw","hiw","bx","zabp","bnh","ba","tybh","gytn","dpc","caf","fw","ghpy","pbhb","wphd","yb","wycn","gybf","ifv","jv","tbb","an","hz","zt","hy","jqv","qnw","fvk","yj","wgn","hndb","bc","chh","wipy","vhw","wb","btv","ytw","nhy","cxaw","iba","zhd","cb","wf","ib","ak","vqjy","nm","kz","agt","znmh","dcnb","dxw","bb","pq","cyph","cjy","bqg","jyvk","yvwc","ydp","hjpw","jxkc","cwxm","bxwp","vj","wv","bqyk","gfby","cy","tb","cv","ctbp","vhzm","bvc","mx","ipz","hb","cww","iza","zh","kqnd","cqw","bt","jc","bcyi","qjhw","hz","hhiw","aiy","bmk","mbyp","jcb","xyzb","bfd","wxgq","zwbc","bi","hxp","wyqz","mctk","dtw","whb","pbih","yxbn","gt","ww","hij","fhg","bng","qpx","apv","niba","bq","fqwv","ix","ykwg","kw","jtyf","nh","zmck","yb","zb","vw","fty","mi","dbt","kc","wyhy","ib","zm","dwx","mwjy","tbjw","bkg","yfcm","fdxm","vfdy","by","zkca","yyg","nwgt","agh","bwhd","yqz","zthb","bwjc","tp","fykn","vhad","qba","ybh","ba","kcyt","vmjh","mcw","bwxg","my","yb","jbbz","bak","ha","hk","bw","yq","gh","vwt","bh","zqyb","dpbv","gj","yzf","jm","dzg","pfbd","ycya","ba","jcfb","ciz","ty","vy","wgby","vqz","wba","gqn","wky","hj","yzj","hht","ibj","ziyn","yzj","jgy","kjgy","pnm","wbj","fdwh","hw","gbpy","zb","yxd","pt","dbb","ixf","bbzn","hgwy","qkww","xmwd","yzw","wbkx","wpgh","cyqh","ch","htk","yw","whyg","pbd","kq","cc","zd","ypiv","xhp","iwz","wcjx","bxm","hh","cn","yy","fy","qhd","xpq","fk","hnaw","jbik","cy","zwb","anwz","ycn","jf","nvty","qcn","wmc","hybq","ywx","nbch","hmb","zybb","ayc","hqd","pby","xyz","zifq","bqw","bp","bhch","bfh","ftz","px","yabh","wvc","kyyc","nmbb","bcy","bvwa","gdxm","ydyc","kdy","wbyf","jqyt","viwy","xqcw","yybp","yhy","cjx","ytwy","bjb","yznb","wq","yw","cnyx","fab","xc","qhy","mpb","yfw","fk","xhna","nby","bgny","bbzh","ckh","bd","zvm","ywf","wzd","zcvp","cpxh","at","pdh","zh","amth","bhkj","zbi","hy","my","gcdb","gdi","nw","xmi","qbd","wwch","mnw","mj","kav","yyth","yngd","bpyf","vy","fid","hz","bh","cb","wydb","jtmh","bgxb","fbgh","chyc","ccdg","tvx","na","gn","xyb","vx","ndwf","ndkw","qb","bgh","bd","ja","kd","cq","wyfv","tyq","jb","dgn","qmgz","xac","cvh","cqnc","kh","qpac","izx","ki","hgb","bvb","vhb","wd","ati","yjw","phh","ccmb","ipbj","qby","jbb","gcbb","cbd","fwbm","dc","ajb","waj","fh","hb","zknb","hkim","dgb","bypx","dhx","zcb","jwf","hyc","hb","ybbv","pm","bp","bw","pcaq","yqah","bbqw","cb","bq","gi","whkg","ht","mb","bq","kf","bkj","db","cb","afmk","igw","ih","wgbw","hvc","axwh","acg","jbnb","ynwa","bah","wygb","phc","pvb","ybgc","ytg","fjc","icy","amyf","yb","wicw","mzbd","cnb","jbb","vc","na","da","hbtb","gpij","ybh","kytc","qa","mbya","qc","ci","hhif","dgbc","cg","ihzc","ymwd","twph","qb","ch","wzhy","tca","xcj","wjfc","wcn","ywz","jb","xqjy","jyzb","xjb","if","ikc","jvc","jbnk","pvfb","hm","hc","iwyj","ch","ndb","dwm","im","vd","baq","tcw","hkhn","vcp","gbq","iyc","pywc","mw","cba","yayb","khn","cjx","zfp","cmg","yi","vmhw","fm","bh","cz","db","kbh","xbdw","abyb","bakc","fgz","qwb","hwyn","xpy","vcjb","zqac","igq","mxb","kbxt","wyb","ww","btiz","cicq","fwm","gvcb","mh","awwh","vx","ytv","mci","yp","jbyi","dc","vb","pn","fh","wny","xt","taq","gw","ig","wn","nkj","bf","bmw","kaf","tnhd","fyht","zc","xyh","hab","zi","wng","wtvj","xy","ic","tifz","thgv","kfmj","xmwb","zhay","mgf","df","by","jhn","th","gk","nk","bxy","vc","bz","hk","ynq","hm","hcc","dbtf","vg","bci","icwk","qtj","kj","xhtg","hc","ykfn","jbky","nwyb","vw","pb","qyw","fy","ydfh","qv","cd","hgb","hww","az","maw","tbd","gcb","bdvi","kcn","znhy","ac","cidn","bthg","qbix","ym","wvq","an","vak","xyib","hcw","cb","bc","fxhz","gq","xbq","bhmw","iny","yh","jbx","mwz","yghd","fdxj","njp","xqzh","bnzv","mzib","qwjy","zb","bpcv","xvp","gyp","hcb","cqz","dbw","ywny","azfc","gzq","ima","dy","fxqt","vp","hcwb","yz","yap","wbhj","aiyc","wwb","yc","cdy","jnq","adg","naq","qyvc","gck","wpn","cadw","wyw","nh","nqa","jm","xyat","qtzc","yd","pwb","mdcz","ybx","ptwh","chtm","gh","xiyt","yzwb","ywz","pcm","wy","wfix","bdw","wfc","ihw","ntb","vbh","hwb","aw","ctb","yp","wdbi","gf","fybh","abdz","hy","zbib","qbzb","hbzy","bbh","gw","hwy","yy","vj","mwk","ngc","ajkx","in","cdjx","hf","bxy","yz","kg","wmg","fhbd","jd","wv","bxd","bvw","ihwc","hfh","wb","kn","acc","hazf","xy","bjz","jbx","yzn","bw","by","iamq","ycdc","tdp","njfy","fznp","wz","akvw","cafi","wym","thmv","hh","ny","tmv","bphb","abvb","bg","wfv","xc","qmh","kxt","ach","cp","iaq","kgy","hdwj","cf","nbtd","qnw","twcy","tnwj","kwb","hhk","bh","mjcw","btm","iyjt","mvz","iba","zjv","twxg","hj","ma","fwh","vg","wzh","wvc","ak","jc","cmik","kwm","fjq","hby","dnw","tbh","ya","qcwd","kv","mbaf","ywwb","wv","gwab","af","vx","za","hw","bjb","bqch","cgnb","bwcz","cwdb","ag","nw","kc","qj","cy","cdw","tid","fn","cm","cnbm","gi","vycc","qb","yk","cwy","tfh","bh","gf","yfa","pymk","yq","vxqi","bq","bi","qmaj","cpdy","ihb","wcx","pxy","tqaz","kbfm","kfcc","yyp","bk","jiwb","bihm","aqjc","cb","vg","nivb","ikv","gc","jcy","bi","vx","xhj","gtkx","ptdw","ihyt","fh","bqi","vcx","bjn","bzdm","mtav","cy","vtbb","ctvw","bnhc","gthk","vc","ykan","jz","xhny","wfh","znw","zh","cnyd","bn","fymn","fnyb","ijf","mvc","df","zwf","ytq","cfi","ybj","mw","cy","htq","hm","pm","jphc","vhd","hz","by","fxb","avnb","cv","hmy","xty","qv","tf","jkfz","wmji","zkq","hwv","gw","hacb","yych","kdn","whk","knp","wzi","bfy","fhgb","hbm","czw","phjy","cb","ybct","hc","bcn","jm","vfw","hbh","pta","tcpn","jyg","ytcm","hbyn","ct","cb","hvp","wbxa","xcd","tahy","ajwh","jb","ywk","ty","azpn","bd","mb","ibc","dvc","bzny","dbbw","iwc","wj","iycy","wh","bhba","tb","qp","pzbm","vyby","gcd","wp","fkh","yjpb","bydz","hb","nb","qft","nfkv","tiy","zh","bqwi","yb","pyf","hb","zi","vq","wxa","ahw","gn","gck","xk","bf","wmx","yz","jhxz","hd","nfh","pnya","zj","vyjb","hhn","hig","iycq","bcq","ci","xibh","fq","gk","yd","bmz","bi","mb","hcbh","hgdy","pch","iht","jvy","xth","bqjv","bb","ik","wy","gb","dkp","wh","wkcf","bi","ah","kht","pyk","hjvq","bgyk","jw","hjb","ipb","ipb","znmj","by","bbb","bk","bywh","jwd","pbh","nfkb","pb","id","fbb","dngy","hyh","xcb","aq","aw","bvk","wc","apg","bnv","hcqp","bc","gh","cc","cy","ti","bh","bhb","mgby","yk","ngwb","hh","zdx","jv","ty","vga","dxj","dwfk","gk","nh","qwhc","bcc","vfnb","nqbc","fi","qwv","by","mfg","wqhk","kaby","pn","ybz","ny","yp","na","gx","ayfz","fb","dwx","hdkc","ycji","ih","kcj","md","ackd","gyqd","zkvj","wz","jhk","bcn","zhw","vz","mwtd","zk","bh","itwv","jc","cqj","bwzv","bixy","yn","ahw","mg","ny","by","wy","bk","vzwf","hj","dyzc","qvht","cxzy","pbn","azh","bwkg","axyv","dbkv","jfp","vzq","qb","wz","bakw","zv","khph","thb","hp","vfk","iy","bcxd","zc","wa","bcd","dn","ay","vcnj","hky","zh","tbi","zty","nvf","bmcy","wyb","dmi","hktb","ahh","nk","nhy","jv","taxb","tzg","hatx","cbn","cwgn","tk","djf","wycm","ca","gixn","cqk","hmzc","vcfc","thay","bd","vyw","djv","mbty","hgqj","kqm","zh","jh","zc","ma","hbm","mdh","myk","mywi","bzhb","xd","cpk","nqvb","ymzc","fpgt","ca","jbyw","vz","zywc","gvbb","qyxh","fd","fap","ktpv","pf","mawh","yjgt","cy","hgib","nqzk","cb","hnbb","gqhb","javk","qdyc","md","ydcb","ihb","nic","hkw","hjb","wpx","cwng","ftm","wkcm","bwqz","jyt","fbh","iqp","ckh","mfzy","xq","fpjx","bi","zd","ht","pbwn","tncw","kag","dykz","wcc","cc","tkpc","ap","hd","cbth","cyat","gty","ajxw","jyc","hiw","mw","dmbg","kc","hwpz","gbf","qchy","xhac","iga","pq","hk","gzqb","hmz","hfxh","htzh","gc","nxpb","jfy","nyd","vwhi","wagb","kt","bzn","vpi","kcwx","ji","cypt","xcjw","dxfn","bdi","hm","byqw","vhb","ctpb","tc","nfi","qcn","nk","wqh","ih","hdha","ba","amq","bgba","anxi","za","wd","ijt","kz","qxbm","hbyx","xcv","wp","mhc","gyc","yw","ic","gp","djav","nhb","anhp","qwzw","chw","hyct","imbk","btn","ph","wwkb","hg","tpfk","zwyq","hcw","xg","ihb","wcf","hiw","gvk","kw","ihzj","dng","kx","qpb","bbpq","nq","vg","hw","pbn","phdt","xj","vbci","fvc","ywq","dbx","cwy","ymv","dbqw","cah","zn","htmz","qpmf","yhp","jg","hzbb","gzxy","kvc","nbk","qf","zc","qyp","bn","pya","at","pjt","mfdh","mt","chyw","cvxw","dz","ax","dqfb","cbqc","amwn","ymh","pfcb","nyq","tic","dkqt","hhb","ncbf","iwx","ybch","nh","ywbb","dgb","jcd","nd","xijn","bagc","jptb","bgk","twxy","ybcb","zqhy","wdb","wzmt","bck","fd","cjbx","bigh","xh","qb","bf","hjzb","cyp","fhw","kcfa","qij","ngak","mhdc","itbh","cbb","iwvb","bi","tpb","ik","pfq","zn","ycj","kz","jcc","ynw","hi","kbc","gkpv","jbt","igh","vjp","bgt","nyyz","wfn","bw","aigk","an","btnb","vp","dc","hny","yi","npdb","hwk","wg","cdv","pbh","fpk","yag","bixq","wj","wnc","cbmc","nx","fm","zh","nbx","gbiv","czh","ybwb","acx","fw","byjc","tzhw","fzmv","tmja","zahw","acy","hyvf","kpz","mjk","jq","ccq","gby","bc","kycw","tchn","fij","gvkw","hmyj","kdah","tb","hz","nbq","id","wcw","zgk","bpb","dbjf","zn","mcwh","bcph","hpc","it","py","pmic","ycn","tvcx","mj","bmj","xbw","cq","wt","bb","vtk","qz","mi","jn","iz","hhqb","cw","bk","bv","tcj","by","bqa","dyhm","bwn","hdbq","pwb","wb","mv","wfdb","ymq","hp","hnyh","wng","awv","kyd","zgq","yn","zyj","aby","jd","hbi","ty","xbnz","mc","bxp","wh","hvz","mihf","tq","jw","xg","yi","py","zqf","ct","wpb","fjqc","yv","wtyk","wjkh","vyx","wahv","bzj","tz","av","dh","qx","kx","nhqk","ihj","xb","id","bi","ygqd","cxm","jk","itcj","iwcz","kyw","bi","hvj","kc","twbj","bp","fjwz","gb","gzn","bcx","ck","zh","yhma","cyip","yvb","tgcz","kbvb","bw","qi","myc","xbcw","wp","ijb","yhv","dka","bwh","mby","kjcw","qydt","ag","wbq","dk","zhv","kb","wdg","bdax","wcb","jn","bt","mwj","fan","bnp","zmf","bmh","bw","qhb","ckam","pcmh","nzc","cm","vq","icn","acyq","qbg","zj","yaz","fnyd","piyh","gi","yxz","qzab","phn","btbd","dbap","pwmc","vtb","bqi","xmbf","fyzn","ay","bcf","ic","cyn","bvj","cya","hc","wmwh","mfwz","mwn","yq","xn","bt","bqd","mjq","bjih","hi","wxy","xig","icdf","mpy","bym","cwwy","jyph","ja","yz","vyw","vbwp","ibjc","mvy","qjb","ph","pmb","xw","dyy","wti","by","yn","nk","cz","wp","ymtc","bvkj","xvwq","nqy","cctx","gicb","mzbx","bgwc","zx","mbb","pj","tvwc","pa","zfja","mbv","hbmh","gb","jvmc","qd","bih","bdp","cf","gjz","cpt","wfjh","yz","wpb","fmyc","yvwb","pc","ahcq","bx","ypfb","dcxm","yw","zbh","yb","ayfy","pwc","vqm","bq","xag","ac","jv","fb","wg","tp","hjiy","vbyk","qpg","bnby","pj","pjkb","at","acb","bct","cjb","wp","xaf","fwt","xt","bqxv","nz","vjbf","cnxp","dz","fb","fg","hwwn","ztp","mgqc","zjw","gcfp","imyj","yt","vyb","axb","gz","jd","yyc","yz","hx","hy","kjx","wnyx","by","havt","hzdv","ccg","dmy","fi","cb","gjax","kb","yw","fh","jhwb","kchi","cdj","nhqb","gbfb","ydfn","byk","nbiy","hapb","vp","zhdi","fky","mh","gqy","gm","ctm","gbj","bxw","hcy","zip","mv","jdhc","zfb","wyc","cywx","twwi","mkf","yd","yb","dmb","hiba","pmhb","fkz","yh","cpz","tip","jy","dm","xbhf","hqzk","fpi","ctz","jhgn","nj","nbwa","ktay","qj","qa","jnyz","py","zybh","xc","qb","pd","mnkh","fjk","wv","hh","fb","cbxp","ay","cbk","zxbt","icax","ink","mxy","ajyw","yq","bbmh","wqp","wgcq","zbpy","ijnz","xc","px","qbah","zmjn","jc","ybp","ct","inhb","dw","mi","nb","vj","bqy","ci","dnbq","ckqb","cqwy","hx","bwqc","iw","qa","bjcp","nyw","tjh","bhzc","fy","xgbw","baj","cyag","jnq","axwh","bxq","mw","jx","hxg","whi","hvjm","qpty","yh","jtv","wtcd","kxw","xmj","fpcd","dmip","zhy","yc","gjb","bbnw","ifm","bdcn","ytp","hz","qp","hw","ciqw","dhp","twb","dwhm","mb","pzkq","yc","xhw","bi","gj","nzb","kc","wq","bh","mq","fqct","yk","jtv","tghb","wyb","nj","pb","bmc","qfwt","ya","xb","xwkg","wzm","yywg","wgx","xcyi","hywb","zafh","bd","ct","htwd","ghvi","bbh","vgc","wwx","cawg","whah","yv","fnkj","vhd","vwhd","cmvw","vhf","bfwn","xh","xbqc","vzbb","qc","jbyz","nw","wyp","bdh","cbhm","wc","yd","tqb","hd","ny","hab","pbv","vhx","kwd","zp","bi","czb","mq","fc","fh","qhjt","wbcd","jbb","vawz","bc","bf","dxy","nya","yhj","qdy","ztp","kn","qw","bbc","ct","pi","hk","gf","kc","wd","bf","tbpz","yan","bw","dt","wx","gbd","ntvd","fgjp","fw","pnt","wgbc","gf","ibkb","cyji","hy","bhw","ihk","hbwv","vabk","zvbh","qid","qf","nwav","hd","hfwp","hcbb","cdh","bqg","btwa","qgy","dniy","chpn","cymz","gd","ih","bphi","wzax","jbdc","qy","gwy","ybm","wb","vha","njha","chx","hg","ytw","wbj","ww","ftm","bphy","mdhn","ift","ycv","dwx","hyvj","mb","bw","wtj","bzyc","yz","bkdh","gtj","nzbk","xc","gqhf","bj","acxh","kiwj","qkg","dt","ybcw","win","wcgx","qxhc","cm","mpc","dkjw","vbya","gp","chh","knf","tbhd","izbh","mav","bhd","dt","wbfj","hd","njzt","hct","ccmi","cwib","cxf","atnj","yvx","bz","aj","yfc","kcb","yy","jmd","cv","btbi","ib","xk","djyv","kjz","ixw","gbj","yw","yqv","ycg","bmcb","pbxh","kgbw","bjbc","gbc","tji","bik","md","th","za","zyc","nfq","tgdb","jfpm","wzpq","pwmc","hyq","gj","tm","wwi","nvkg","gbw","pcb","yby","kbqw","pbxf","qd","ky","yc","wx","cgtf","fyw","ydv","hidb","cvc","py","cg","bqn","hyt","vywc","xd","nbb","pzcd","cbmi","py","xinw","bbnh","hqvp","izm","wc","awt","hc","zkc","wydw","gcf","dphx","ibnj","gdzm","bqzw","wx","wk","jn","vhb","ki","tkbc","yqvh","pc","jyxh","dhy","cihx","bghc","xbq","iy","bdf","yh","by","bbkd","itqb","zqdc","nbt","xygf","cyqh","dpzm","wgn","ihyw","tn","nbp","wd","nyx","ty","cqnb","wa","vi","zkb","nxfi","hw","mt","bbyc","vhb","tczw","zm","cctz","ib","nc","iqcv","fyv","wx","ynt","fyb","pbcf","fbch","mzh","xf","kb","phcc","gcm","dh","tbf","nywh","by","gybb","kjta","wkx","dnq","ct","anfc","mbjf","ch","jbf","haj","txm","xkmj","fg","qmzy","dg","kzha","tcbk","fc","hbgq","dp","jv","qn","hd","xqab","dc","cthy","wjyw","jp","dw","hn","bwkf","wc","bwxv","zx","iy","bj","cz","dba","zv","jnt","bc","nch","dthy","wwn","ziv","fy","xk","aw","cybm","tb","pha","nv","bhtp","zty","bqt","xy","wkj","pykf","cwzf","fyad","ptw","vwa","pqyd","mxyz","whq","ihxn","dygp","chdi","bvwk","dw","tywk","zmw","ic","xn","ha","izam","yay","cn","cfq","zcdh","gi","ktcg","pb","fchp","bwq","hav","vhk","wnq","fhw","bn","hcdv","qx","mfhh","kah","ybwa","wbt","tpm","hm","ca","xkp","ypiy","fwk","fbw","zc","jx","nyg","gyx","xza","btcn","jzm","hcj","yimg","yj","wht","jqvw","tdf","vc","tih","nv","wm","wib","hmwk","hm","kz","bbg","tb","phw","mbb","bv","zhwa","caph","pg","bv","ixcq","akp","abbq","qwc","wgv","jct","wi","vtb","dfzx","bqcv","ycvz","png","tvw","kx","ycba","hpyz","vyh","ab","ywg","nyq","pj","ba","bzh","bxgm","xknb","gbb","pc","ygc","hzjy","bwvk","bnwh","ydcb","cwtb","ajh","gqk","xbn","qwc","ygv","whnw","zb","ybb","jdy","hd","aw","gh","ik","wb","tbbw","zjtf","chb","qw","jtpx","ja","qg","jwhx","ptkq","xzjb","kdmb","zfpx","twh","cbg","dk","yn","pjy","bt","bfd","jfb","xibg","apft","pvb","nw","jiyh","xthk","wbmb","bt","zv","bxqw","pc","vjhz","jdfa","czbf","kypx","ybcx","mc","gwyb","bciy","jdzh","va","by","kzgh","kp","wc","hjax","wiy","cy","xd","bcyz","yc","izwh","whfy","vy","wzcm","ytwk","bc","ynbi","mcy","bq","gdkm","dcig","tmhc","dv","wtqx","db","mbiz","itc","tx","jc","hh","wb","vpb","wtyb","hytj","hvmy","bzb","qbkg","zk","ib","ihd","jgth","zc","nh","cjha","byhz","wtbw","dpgf","qg","nxwc","mqj","ftcw","aphz","hc","jqbv","kmdc","zy","cvhc","cnbm","mxyc","hdyw","cpb","zfw","bid","bcpw","mn","zxb","wmg","gdiv","ihn","nw","cbhm","by","bh","byaw","mv","hgp","qyb","kjf","djb","cy","di","vbz","kmc","nq","xai","pwx","hib","zqva","tb","hviz","cb","cxtd","kbhb","czbp","cn","ckc","db","hta","pcyy","bdn","bycx","tbdy","whj","fqch","cq","qch","nbcj","vp","hhd","fj","vnp","by","wyjg","bwbc","dp","xmyj","pzty","yz","yk","dnp","jy","ind","bzc","xzwj","qj","pjq","niw","jb","ybd","tb","by","hj","mxcc","cp","dnac","zmx","cp","fw","ym","cbq","mwk","ybqc","bgcf","qia","vky","qbn","whkn","hpym","pbcx","tph","gbyw","yj","igd","xvbc","gbyx","nbb","ax","gfh","hw","cfb","bah","adbc","biv","cvd","fkdn","difj","kgyz","jzcx","jh","zy","qygt","ww","xcng","bp","hcxv","jd","kbtw","zm","km","zc","aywy","taj","chj","byty","gfbk","czd","iqy","iq","jc","px","hzb","hkin","vxap","pb","xby","hdbq","wkj","bwgb","pbk","mh","bf","ynim","cgp","ta","zw","hb","gqbi","xa","ahqf","hg","vx","cv","xgvt","wb","wbfc","kj","mww","pgj","yxi","if","fybv","bp","tcpw","ygp","bt","ch","wafq","cf","wbh","fgy","bz","gqp","fp","ywxg","bqj","hzw","gbh","bq","bk","bh","avjw","wvn","qm","mn","bzch","kyvz","jbdg","bt","bbt","tp","qgcd","yf","nb","bvxw","nijc","fib","by","cf","ybc","hmby","bbkn","pwj","wj","payc","wyxb","nciv","ndy","batc","tba","bh","cxw","zmfn","baw","mwp","ivw","ihzg","ht","dc","mxa","yjw","ny","xbpg","pfbg","hqck","cwjh","mpw","cwv","kzym","kbc","dfc","ti","ibb","xwpz","hhj","pn","jwaq","aix","iqy","czh","vpqg","hk","pbyi","hm","iyz","bd","fj","ykcf","xhv","cmva","czbq","fqct","icf","icvx","qkd","nzw","myiv","fynz","jcyh","kivd","kixg","cap","mcbq","bh","mhv","dga","nj","jwcy","ykwq","qhkc","bbby","mkb","fzn","hqmf","nhh","whdm","yz","ycap","by","zyxg","hpjn","gq","dzw","qp","cwax","bi","zaiw","ycgz","tfz","gf","wkdf","px","yj","cyh","hzjc","bnwc","mc","gfh","kx","ktvw","ytq","ydpc","kcbh","cahg","jihq","knt","vya","hww","pbyc","qwgh","yh","dwb","mwx","jqt","xbji","mbh","wc","mn","wwp","whvd","qti","pbbv","vnbx","ahd","wwgk","hxnv","kyb","pjfx","hf","jdb","cqc","qnk","cbd","qbb","hij","tm","dgcv","ft","yi","xicb","ybw","wyic","hkz","kyb","tyw","mqwv","bw","qd","wbat","nfhb","jb","wxfb","bkwi","ab","zn","nhj","yhhb","cvb","ymqz","dywb","cxqb","ib","whqv","by","pyh","fh","wkfw","bti","bb","yj","bc","vkyh","pvh","dmh","vbky","pq","vckd","pc","jpym","hz","wmy","xg","ycwb","ywb","knc","cj","qcbi","mh","qj","mdw","iy","jgc","ycjb","kfc","qxpj","fzb","wfb","nmh","vk","ajmy","xcng","xg","ifcc","tnq","wxwc","cm","fqyc","yb","yv","pcgw","ivbj","bbnc","qt","wzb","hbg","qh","iy","wz","wcg","awtw","hcdw","yx","wyv","qt","cjp","pix","vjy","qz","vch","gy","vkz","dh","jzh","gp","mxw","wkc","tbzy","bwmp","bccw","zhwn","bm","wdjz","avbn","ibj","htxa","mdy","kghz","pbn","thy","jv","pv","vbc","khm","xg","kf","bgqw","gh","twj","gv","zwyg","fmg","pc","dc","pfc","dfb","nd","hvb","wzc","qh","wh","my","wc","ig","fk","hbjb","dckh","ambz","qb","ib","ynaw","gbyc","ktz","vwh","bbgp","tw","xqb","qwb","ik","fb","bh","btf","kj","yd","bi","vb","bd","cb","nqmd","fjm","mybt","yif","hgdy","bb","ch","izma","vxh","wphc","cbyt","qd","fd","bxbh","bkbv","cm","tyh","hf","jk","tq","gbmw","qkc","yp","vnxw","xiz","bkzi","tpcy","ajy","yf","jd","mt","mk","cnx","zbwn","cgk","pnwb","bp","wfjm","hyxb","btx","qh","fab","jcm","dqpm","pdf","dth","jdzm","qtfp","iy","tp","ih","hfh","bwca","cbb","zvbh","wtcn","kc","fyw","zt","tyxv","bwkq","tc","mh","vng","hp","bkgm","tb","mw","wy","bcdv","pf","cfvq","qand","igb","hwp","tzc","cfy","ch","vach","dv","mx","tn","hbvx","hmx","inz","iadb","nwb","ydh","bgc","hkc","fkw","bwy","dbjc","bww","tbpa","ika","twk","giy","pcw","tdym","tm","vch","dxyb","ta","zb","pb","tj","cb","kcw","tiqk","kcd","tb","wbq","dyga","ybj","yznb","nf","xkwh","zt","mby","vpm","wdwc","kb","bdm","cd","chgv","tjb","hnw","ac","hcz","hb","qhtw","dwgc","bny","dw","yq","bx","xi","xbc","aq","aw","hnic","tgqn","xgh","yij","ybkw","ba","iqcj","ghyf","qbgy","mw","twjk","vy","ghb","hbc","pk","kb","bcq","czi","tc","dych","ikvh","fb","twbb","bktf","tdnb","kbd","bahv","mct","zx","ghtn","bq","dwbf","tm","xyb","jkcb","mfgq","aif","ny","agzv","mxj","zvy","cyia","dqh","qbc","hw","jfb","bqh","ybm","dkm","xbi","qpxv","ch","mfd","nfy","dc","at","xwmw","bc","wm","wm","ihz","vq","wb","byit","pc","mi","xwk","fc","njbc","hdw","ki","ctbj","hqb","ypht","ifh","hdzt","hdbb","hc","gwhc","mhnj","bh","fm","wbvi","wh","wa","yfb","gh","yd","iyvm","ntzx","pfx","cifb","zwac","bb","py","nykb","cdb","kbah","ckn","qp","gca","hx","pcxt","gmy","awjf","zchh","hbg","wbcx","dnpx","zcb","yj","ghbc","hdxp","vhh","zyc","fcwt","yybt","hxb","dz","tjgv","mt","yw","yc","dpc","pz","ytf","kb","cpfc","mdx","bb","ib","xkfn","ja","htc","mgk","hbhv","mxyb","yb","fcb","iw","mf","ndh","ihbf","jkzt","dxwp","ztn","vh","ma","picy","pnb","wnp","wxaz","fbkh","vkhi","cqk","bhgw","dchb","mhcg","vw","bym","hfh","bc","wm","vi","wq","mbdh","mb","whh","caif","hi","hc","jahb","ig","abx","ab","ca","ngvy","bd","md","tgij","cnky","aktb","tbhp","why","ajzc","kg","hw","cp","bqv","vt","bwza","bzh","hq","ty","fbi","mhp","xgq","jxww","iw","gw","nh","aw","nyx","da","cbd","acdb","jywk","zwb","zb","vakh","yqf","xay","vc","bhx","fqn","dyt","tfwj","pv","cb","xcjc","wg","pa","dn","mk","txy","cbyg","bbww","izqn","cyb","abnw","bvmf","xd","tpv","pcmb","jnh","dnjw","hcj","hj","npk","hwzc","mdn","mwc","hb","znhw","aw","hca","pcq","ighn","bcb","cwh","ab","kyy","bmt","cwf","wmt","ht","cgwi","cw","zhbj","ta","xhkz","wm","dh","pz","mbnp","qzn","qt","wd","nzkd","itkn","hym","ncza","wx","xib","fnb","gbbi","cb","qpb","hqy","fgx","xfn","ct","jcxb","mabw","yz","bph","dc","hbk","bpy","wm","iyy","nzfw","abmh","mwj","ywy","gw","amnv","tbh","ztyb","hcb","bmfh","byzc","fb","hayg","ypnb","xm","ihyc","yqf","ad","cw","jqah","kd","xh","xqb","jbvh","ghbz","mbi","it","abh","jzim","hgbw","cg","bbjp","mcwi","nykf","dvf","ai","wt","cfja","pq","yxya","wdjb","bb","dtvb","wpm","iab","av","tyf","ayf","ijh","hhpk","cyb","wnh","bz","vy","wphi","xt","mvd","nb","xbzt","mdyf","pbw","iyc","pk","dwq","wbaz","bd","cbdx","vqxg","vkb","tmbz","zc","cz","ykxj","ma","gx","vzjy","vhy","zhw","zcbw","bqwv","bkqm","czn","yqtg","xzq","wmbg","qdya","bwh","kbyg","cit","fnc","wv","dkn","bmkw","fpnx","fqbw","znga","fj","nzgy","zq","gnkb","pwh","wkgd","hfh","hx","zmvk","tqbc","hqg","cawh","wgz","bd","gb","kbgq","nhcq","qndk","mykh","cbn","bmf","bwh","gb","kdj","pbbc","cb","vqw","ybkc","zjg","yf","nv","yz","bwyg","acyw","wh","yyv","cpb","yw","bvpn","mkbj","xaw","zcyh","ygdw","kjxf","xfky","byk","mht","xa","yy","iywa","zibh","wizt","fw","my","zfk","fcc","nhvh","ndc","ygmq","bbp","af","jwf","hvb","gwyj","mvdn","wcyx","ckmp","yakn","mdnk","cpm","hpy","yb","vxk","cy","xcb","mby","vw","fyp","ck","by","fk","xbwg","cjnm","tbx","ink","zm","tvh","gtk","zip","pihb","qkp","bij","qkd","hcw","znha","cf","nwqh","icm","bynh","cabh","ydm","cbc","xc","whij","yn","zdht","fjgw","zgi","ib","xc","iyg","vc","hj","cb","yb","bxwy","hf","hb","fyz","gbh","gqnz","tv","ng","qfdb","jpd","yhvh","qb","hmj","iyz","cpbd","hk","bid","ywv","fjwb","ctbv","cizb","wx","jpww","bj","dp","bwyb","vb","xngv","nfph","wi","ibbb","czkq","iqzt","ty","ty","za","bq","bqi","wcd","wbw","gzx","zy","ixw","atyb","bhk","yqp","ybkp","ity","jb","fw","bc","fciw","mw","cdp","khw","ag","wcn","jb","jyx","kah","wb","mazc","bmj","wb","bp","hfyz","tyiw","xhw","phh","hpwh","hict","bjpi","bbwg","nya","jqp","cw","gwb","bxyv","jdt","txa","bn","apq","mt","avm","bkd","pyc","bc","hp","fmaw","gpd","bwg","bj","hytc","mb","hgp","cybz","nw","fc","wx","thk","xhdk","qjv","jyb","hc","gbnb","ayb","bjg","nbz","kif","cn","cmyd","ftk","ygv","cxgh","jncp","fcq","nca","bth","bmj","wj","wvyy","byi","kzbc","nqt","ctk","wh","af","bmbw","qm","hbpf","hvqj","zq","fbn","wvba","bwtv","zt","ni","chzd","awby","ifbh","bhv","qw","tkh","mh","hmb","hv","cnbk","dfba","pdwc","gacf","hy","ct","pcwg","yqk","gamt","cp","idyy","gvy","zbdq","bbc","dbi","bjc","bh","bvz","caq","mv","qgz","vhh","bhc","ptag","ymkb","kh","tbji","vd","wah","wfd","jc","wn","phby","bcm","hcb","jyn","fxm","tj","cwp","cd","hwdw","gdcb","nc","qkb","hbfv","xg","ghn","wykf","fh","xbty","awz","iy","wcvj","zb","qc","ay","fx","wpfy","abxm","fbb","xa","whpd","yh","bwiq","bnz","qx","bya","iyap","knb","fg","hjb","xazd","ghm","hihb","wc","kbi","bc","hw","qbn","jpbz","ki","bz","kdbc","ac","mw","zpv","yxpf","yc","wbmc","xdh","id","qb","bbf","hb","vw","gj","dy","bh","bvb","pw","px","hwbb","vz","my","cg","aith","pqmv","gx","bb","cxfq","yxmc","dczy","jawh","gky","kcy","yacm","kwca","btx","hi","cip","bfcv","mykz","xcb","hw","yic","qzi","cjih","md","bccw","kp","zkyq","qhym","twp","pvc","mbn","bpb","ygpk","hn","vq","jym","yhk","fxc","fvky","kwnw","hbag","mb","jh","xytd","fi","vnqc","yjzv","fmhv","wp","cbpq","wgc","cf","mb","fpt","pw","zha","zajc","mbq","hp","cwp","mww","cmt","bbk","fmgv","yy","bh","tg","zy","ynfy","nbx","tdy","cw","ydac","tf","cny","wcf","xp","nmxt","jyk","yw","ibxj","yth","bh","xc","an","iyw","mhad","zbwm","gahp","hy","dxhj","hwc","cq","mihw","nyvw","ty","dwh","bc","xpg","aij","yf","im","vb","tzc","hgyj","id","gn","bg","vng","wxyz","zw","xci","tyv","kgv","cb","caj","bpvy","qa","nx","hawb","ady","mg","wh","ztm","wph","nx","gqzt","id","cbjm","fbi","zcf","dvjk","azf","qyv","tq","vh","fvyw","dnyg","pjvy","kdw","zfx","bch","agd","jnmd","ad","bfx","zjbd","aw","tvg","vz","za","yf","qy","vmq","cwb","vpg","pawy","pqbw","bw","wmt","bjt","hvcg","ykc","gb","idm","xp","yh","cwxb","fb","fj","bt","hfm","pbhf","ny","aw","jgm","px","wat","fd","pw","cbb","ydkc","qh","bbc","xgi","xmbb","xm","gqb","aw","yzkd","tmc","cta","wbyp","mbbk","wnm","hadv","vgpf","tzqx","cpy","wqgc","ymw","njvd","wy","zbyh","vdyc","cnp","kcgf","dx","ht","gawq","jkb","pww","gk","tbab","bbad","cbwh","my","hdw","xm","bvy","yx","byp","dag","vghm","bvic","hc","vnq","bpy","cmtv","hzkw","ihkf","qvyz","ypmt","qad","qmh","vd","kan","ygw","wnm","xtwq","fzc","jp","yk","bnw","wcw","ajzc","dyiq","yhgw","xidy","wbbi","ccy","wi","ga","bb","gmk","fj","gwbi","wpb","wx","ca","yc","nq","hfxy","bqkb","wihz","bbvc","hg","qtj","dnw","fqh","cg","bc","tc","dp","xcc","nmpb","tg","yb","nv","bt","xmz","ptwa","cbp","xb","hti","cm","nvt","fyw","tyhw","wc","ww","cbb","gky","ihwz","bix","dycn","ht","cyxd","hqyn","ybig","pz","bd","xkfg","qk","hzq","qyw","ybyg","wbj","ab","kpqn","ay","ct","ct","wdfb","aymd","vja","yv","cbcm","kw","hwnk","xnb","fwda","mi","pc","vaww","hzkc","bb","fvb","ykc","vpb","ayti","xdqf","jfcq","gb","bn","gpa","cj","akiq","bp","gyc","hv","dy","icw","ybx","bj","qzc","xzby","bthj","jck","gw","jy","bfwj","cy","hwp","hb","vc","ng","patn","cajc","vhpw","kmbd","dthy","ybi","nv","awb","hpab","xdq","bct","dnh","bxnh","bb","hbb","igyd","vx","wgv","ign","qb","wbih","axic","tgf","zdvw","yb","ah","zjbh","iwyb","gdvw","iwf","dq","wyy","ihdk","qcx","wh","vcft","ny","byhj","bb","yych","wbky","hyw","zcdt","hbh","mwcq","hamk","xat","wib","kwb","yiq","ynt","td","kyw","caw","bn","dgy","hkb","wbx","mjq","vy","mf","kw","bhw","cawg","qgc","jk","bgn","wixa","ybh","zm","wbj","bhhc","hxpy","nh","hi","dk","am","khmf","wdan","wafg","bh","yytj","qhgw","wkt","wz","fay","byyk","hn","wwz","qpz","npb","xw","zpc","jdm","gd","xhcy","bgdp","fnx","kw","yjfw","wxc","kc","awg","zpb","qpdh","vb","gc","bbc","cpbi","bb","vnxb","bq","cbhw","yc","zqdc","aiwy","xjn","fmcb","bd","tgb","bp","hf","afd","iach","bftn","pji","yqm","nf","an","ykwm","izy","yn","fjy","jw","hj","cb","wghi","wp","tzba","twy","kfgt","fcy","npmk","kcit","mcqx","vqt","wb","cgwt","ztw","byp","hcd","bdhp","pxm","bycw","zjfh","kaj","zyg","jgcb","cta","dw","bby","fxzb","hwc","mt","qcz","zn","bwch","tik","yc","cha","jw","ibn","bh","bh","ayg","ywm","xvbc","dcjk","bwx","piw","dxcf","cn","wp","vpc","jw","wgh","hym","tgc","xtw","cg","hy","byc","akf","icj","yxb","ahyv","hcn","bh","akv","cy","jnc","awm","wkc","wcpm","wn","hq","tpb","yc","vcm","ta","vcwg","mbwq","knw","gij","zdyi","ybi","nwt","bfby","pbac","jq","kdy","bhd","kj","qbcj","qwmg","ihnb","hyty","jy","bv","cb","hcvz","zbfv","bgw","yv","zqth","wca","bmc","dfby","nt","in","ji","byj","bjz","vqmb","wb","ztd","ghky","xbg","wcb","bpwx","gy","czbx","jy","zn","bn","wzy","nyf","hvhg","hg","jah","wcb","dqf","cjb","bft","fa","ac","qpyw","ny","ypc","bih","bbw","nbmi","qbd","byfp","bb","ja","ichv","bn","wbdp","cpwc","hzid","jvg","vcqf","by","qmb","kbxd","wn","cdk","wqng","cpiv","gwc","dq","bbn","hgmk","fb","bvpg","wtkj","cbpw","pc","bkw","zh","xiy","whb","vcg","yw","cb","hhf","qhk","qbcp","djv","yqic","hv","yhbp","zpc","yy","wm","nxc","zcmw","af","zb","ck","dv","agin","gwyb","dxnc","gwm","hq","hb","kd","pahf","kn","wcz","wb","iygb","yhng","mx","pjzi","vy","tjb","bvai","gih","wgvh","kbzq","cpzy","yy","tbqm","ndz","twq","wj","mfb","kt","jkxd","dbp","zh","bij","yjqi","bwph","wyf","ybw","qcw","jyd","kp","tiwq","kd","hvhy","xbvy","xg","hw","pq","vjfb","xcib","vfgh","ywvn","ptqa","hqv","bz","pti","ykf","tj","wi","ph","ip","wi","bvw","wbh","pdy","ci","jt","ihyp","gqd","kfzw","db","cpab","ywf","tz","wwby","tnb","bwh","kpdt","bxyb","fjn","cjt","bhy","bv","mbfi","hq","zcah","jxmh","phbn","cfh","yz","yiqw","xhmt","yh","hp","zyji","yqjf","babp","fy","xq","nq","diw","kih","hxb","bnd","imw","wcak","bcwv","qy","txib","jxic","ycpj","icb","cq","mbp","thc","dck","zqa","chdt","bk","wcd","kvhz","tkh","yk","zxhv","yky","fhnx","pyqb","cmyw","xby","cji","amfq","fbtz","bb","yz","hnd","qxic","wn","nbdc","jwcb","nwp","bpb","wbty","bnj","hbg","bw","bkh","hfhx","fynv","ty","bbx","mdgi","yc","bdc","zwnm","bwx","qwht","htg","gy","cgm","xqb","bt","inyf","cah","tamg","kbvh","zj","vfc","wh","xht","ynbc","jpqf","xky","bj","tc","bcx","hqy","xc","jaq","ny","qhzd","pxj","zdc","thwx","czcd","njfc","qdzb","dhcf","xq","dj","wyva","cft","vybj","vt","xcv","gmh","mgyy","bv","mtqc","xhp","gbw","ywmb","yaz","gfqj","nbht","ycmh","whzv","iny","vj","gwb","ayy","myb","cy","xtmc","qc","nx","dy","wyxh","vxjd","tyzn","bpw","kcf","mka","dab","cv","khqp","ci","tqhx","ma","jchy","mj","vtw","mhf","mwch","cvyw","cba","cpzx","kzch","yw","bh","wxj","cbnw","zg","mck","qw","dij","xb","qb","kyb","tvw","wzk","by","nx","bhym","jdhn","yhf","kjb","jb","kbbb","yhkq","idyn","zdmh","bjp","pxhd","bi","bqh","cvct","ckmx","bjp","bkb","xma","my","bzwg","bjy","bixc","bczj","wnw","igb","jyhc","wf","bw","fxi","pba","nmfp","ca","wdy","cacj","wnh","ahvi","hygm","vhqx","vwb","bfjb","fty","bp","qvyp","pibc","jkyd","btk","bmjt","bmtj","fk","by","avdm","ht","wibw","yhw","hng","hxiw","bchk","dw","bkc","htpc","icj","bc","hdyw","yicb","iccz","gk","bb","dx","gkw","wwba","nctz","jb","tnbm","hng","cq","wgqb","jywq","zq","xh","ndt","kdc","aqp","qgvd","dc","kpai","cj","zvg","abd","gxv","zbwc","mby","cwht","xvnz","yh","miyz","xbpb","hatk","ywp","wbyy","xv","vjib","ynx","ab","cmpj","gx","bx","cwb","hi","va","gj","fbq","vywz","hfcw","cjbz","cpid","dkb","wwhb","abjz","nt","yn","nbwh","dtki","bhcp","yzv","hnvc","wbh","xi","ahf","ndyb","dccy","cvij","mvfd","qb","fk","hfaw","bqcy","bf","mfwz","cy","qaw","nqa","wjz","hg","gb","bidp","qz","tbf","dgh","xwf","cac","vpm","xw","wcp","fndb","khcg","baiw","hyfb","kqt","hct","hdq","bhc","thwd","cy","zgkq","wd","cymz","gj","yb","dt","bqzb","th","qbz","hk","cp","cb","iqbd","bnac","ab","bq","xbh","bhyi","ydc","cb","yci","nbc","pc","mbha","vzg","jz","ivcz","fpyc","kn","dxzc","zdc","fkh","bnhh","wtca","cgbv","xk","ybjp","mtw","tw","qwy","iqx","ix","yb","cwf","hyb","gph","hjv","jhyh","cv","dhi","pccf","qb","jy","qydw","bkcx","mczc","hidn","fyt","bhpy","yzn","jdi","zhgx","gkb","bg","vmp","xv","tkjc","vg","mfy","xbqc","wvt","ityx","mv","bqd","dcb","cdh","hb","bb","jbp","tq","vkp","bgcy","cjq","nbft","ivth","pwv","fpt","ng","wmqa","cwbt","btyc","pd","whjd","kayh","djq","vap","ki","ypx","zy","xcy","tvbb","tn","pxyk","wamg","jfby","qhb","yhb","vzq","gz","bwfa","dcb","fxat","mc","kh","nd","bqtw","hfy","yxm","zgbx","pimh","atj","yz","cxzm","bc","wc","bhf","ywt","bwz","jfv","dbc","yb","qci","amyd","tbb","yn","hn","tmc","awh","iwdm","gi","qg","cdb","pi","dig","zbw","fzqa","fdbk","aqt","km","ki","cbzj","zt","bcim","ibz","gby","hgp","whcx","hg","xzf","ty","ykm","wng","zqcv","khyb","cya","bh","cbyk","wbqg","ipwj","agp","jy","im","gknc","qy","ht","ygwc","ngbh","mn","qthg","yt","ncht","tckx","cyb","xbj","yhb","hmh","hcky","ad","mpg","ywqa","biy","jq","cgdw","fj","ptkh","gn","ybvk","xkmc","ygq","wj","taq","wa","nb","jpv","nhw","dywq","xyhc","dqc","wmq","jvgd","yf","wb","bxic","jkmp","zihh","yqa","myjc","kn","wp","ad","cb","wfpw","td","jnb","wb","jznt","xw","zb","wy","jt","jzw","qb","gyh","fbmd","tkig","qh","wbm","fn","nwb","bt","kchb","hnwy","cgb","wnzj","yb"], 4616)

```


```python
# Python 3: use hash table and heap, take the order info into account, don't sort words at first, check 
# everytime when poping
from heapq import heappush, heappop, nsmallest
class Solution:
    """
    @param words: an array of string
    @param k: An integer
    @return: an array of string
    """
    def topKFrequentWords(self, words, k):
        # count the frequency of each word, O(N) time and space  
#         words.sort(reverse = True)
#         index = 0 
        if k == 0:
            return []
        hash_table = {}
        for word in words:
            hash_table.setdefault(word, 0)
            hash_table[word] += 1
        print(hash_table)   
        
        # convert to top K problem on the frequency values, O(NlogK) time
        heap = []
        for word in hash_table:
            heappush(heap, (hash_table[word], word))
            if len(heap) > k:
                temp = heappop(heap)

                if temp[0] == nsmallest(1, heap)[0][0]:
                    heappop(heap)
                    heappush(heap, temp)
#         return [heap[i][2] for i in range(len(heap))]
         # reorder the output so the highest frequency comes first 
        print(temp)
        heap.sort(reverse = True)
        return [heap[i][1] for i in range(len(heap))]

```


```python
Solution().topKFrequentWords([ "yes", "lint", "code",

"yes", "code", "baby",

"you", "baby", "chrome",

"safari", "lint", "code",

"body", "lint", "code"
], 3)
```

    {'yes': 2, 'lint': 3, 'code': 4, 'baby': 2, 'you': 1, 'chrome': 1, 'safari': 1, 'body': 1}
    (1, 'body')





    ['code', 'lint', 'baby']




```python
# Python 4: only check if there are words that have the same count as the last element and have been popped  
from heapq import heappush, heappop, nsmallest
class Solution:
    """
    @param words: an array of string
    @param k: An integer
    @return: an array of string
    """
    def topKFrequentWords(self, words, k):
        # count the frequency of each word, O(N) time and space  
#         words.sort(reverse = True)
#         index = 0 
        if k == 0:
        return []
        hash_table = {}
        for word in words:
            hash_table.setdefault(word, 0)
            hash_table[word] += 1
        
        # convert to top K problem on the frequency values, O(NlogK) time
        heap = []
        for word in hash_table:
            heappush(heap, (hash_table[word], word))
            if len(heap) > k:
                heappop(heap)
        # add possible values with the same count as the smallest count in current heap back 
        # in case their word comes before the one in current heap 
        
        # smallest count 
        temp = nsmallest(1, heap)[0]
        
        same_count = [key for key, items in hash_table.items() if items == temp[0]]
        print(heap)
        print(same_count)
        if len(same_count) > 0:
            while len(heap) > 0 and nsmallest(1, heap)[0][0] == temp[0]:
                heappop(heap)
            same_count.sort()
            for i in range(0, k - len(heap)):
                heappush(heap, (temp[0], same_count[i]))
        print(heap)
        heap.sort(key = self.by_word) 
        heap.sort(reverse = True, key = self.sort_key)
        return [heap[i][1] for i in range(len(heap))]
    
    def by_word(self, item):
        return item[1]
    def sort_key(self, item):
        return item[0]

```


```python
Solution().topKFrequentWords(['a', 'a'], 1)
```

    [(2, 'a')]
    ['a']
    [(2, 'a')]





    ['a']



549 - Top K Frequent Words when the file is too big (Map Reduce) 

Find top `k` frequent words with map reduce framework.

The mapper's key is the document `id`, `value` is the content of the document, words in a document are split by spaces.

For reducer, the output should be at most `k` key-value pairs, which are the top `k` words and their frequencies in this reducer. The judge will take care about how to merge different reducers' results to get the global top k frequent words, so you don't need to care about that part.



* For the words with same frequency, rank them with alphabet.




```python

```

550 - Top K Frequent Words II

Find top k frequent words in **realtime data stream**.

Implement three methods for Topk Class:

`TopK(k)`. The constructor.

`add(word)`. Add a new word.

`topk()`. Get the current top k frequent words.

If two words have the same frequency, rank them by dictionary order.




```python
# Python: use hash heap 
# http://hankerzheng.com/blog/Python-Hash-Heap

# cmp to key : from python 2 to python 3
# https://docs.python.org/2/howto/sorting.html
def cmp_to_key(mycmp):
    'Convert a cmp= function into a key= function'
    class K(object):
        def __init__(self, obj, *args):
            self.obj = obj
        def __lt__(self, other):
            return mycmp(self.obj, other.obj) < 0
        def __gt__(self, other):
            return mycmp(self.obj, other.obj) > 0
        def __eq__(self, other):
            return mycmp(self.obj, other.obj) == 0
        def __le__(self, other):
            return mycmp(self.obj, other.obj) <= 0
        def __ge__(self, other):
            return mycmp(self.obj, other.obj) >= 0
        def __ne__(self, other):
            return mycmp(self.obj, other.obj) != 0
    return K

def cmp_words(a, b):
    if a[1] != b[1]:
        return b[1] - a[1]
    # if counts are the same, check the alphabetical order of word 
    # 
    sorted_list = sorted([a[0], b[0]])
    
    if sorted_list == [a[0], b[0]]:
        return -1
    else:
        return 1

    return cmp_words(a[0], b[0])

# define a HashHeap class 
class HashHeap:
    
    def __init__(self):
        self.heap = [0]
        self.hash = {}
        
    def add(self, key, value):
        # add key, value pair to heap 
        self.heap.append((key, value))
        # update the hash[key] value 
        self.hash[key] = self.heap[0] + 1
        
        self.heap[0] += 1
        
        # main the heap shape 
        self._siftup(self.heap[0])
        
    def remove(self, key):
        # get the index of key in hash table 
        index = self.hash[key]
        # swap the key with the min of heap 
        self._swap(index, self.heap[0])
        
        # delete from hash table 
        del self.hash[self.heap[self.heap[0]][0]]
        
        # pop key value pair 
        self.heap.pop()
        self.heap[0] -= 1
        if index <= self.heap[0]:
            index = self._siftup(index)
            self._siftdown(index)
        
    def hasKey(self, key):
        return key in self.hash
        
    def min(self):
        return 0 if self.heap[0] == 0 else self.heap[1][1]
    
    def _swap(self, a, b):
        self.heap[a], self.heap[b] = self.heap[b], self.heap[a]
        self.hash[self.heap[a][0]] = a
        self.hash[self.heap[b][0]] = b
        
    def _siftup(self, index):
        while index != 1:
            if cmp_words(self.heap[index], self.heap[index // 2]) < 0:
                break
            self._swap(index, index // 2)
            index = index // 2
        return index
        
    def _siftdown(self, index):
        size = self.heap[0]
        while index < size:
            t = index
            if index * 2 <= size and cmp_words(self.heap[t], self.heap[index * 2]) < 0:
                t = index * 2
            if index * 2 + 1 <= size and cmp_words(self.heap[t], self.heap[index * 2 + 1]) < 0:
                t = index * 2 + 1
            if t == index:
                break
            self._swap(index, t)
            index = t
        return index

    def size(self):
        return self.heap[0]

    def pop(self):
        key, value = self.heap[1]
        self.remove(key)
        return value


class TopK:

    # @param {int} k an integer
    def __init__(self, k):
        # initialize your data structure here
        self.k = k
        self.top_k = HashHeap()
        self.counts = {}
        
    # @param {str} word a string
    def add(self, word):
        # Write your code here
        if word not in self.counts:
            self.counts[word] = 1
        else:
            self.counts[word] += 1
        
        if self.top_k.hasKey(word):
            self.top_k.remove(word)
        
        self.top_k.add(word, self.counts[word])

        if self.top_k.size() > self.k:
            self.top_k.pop()
            

    # @return {str[]} the current top k frequent word
    def topk(self):
        # Write your code here
        topk = self.top_k.heap[1:]
        topk.sort(key = cmp_to_key(cmp_words))
        return [ele[0] for ele in topk]
```


```python
p = TopK(2)
p.add("lint")
p.add("code")
p.add("code")
p.topk()
```




    ['code', 'lint']



#### Bloom Filter

556 - Standard Bloom Filter


Implement a standard bloom filter. Support the following method:

* `StandardBloomFilter(k)`: The constructor and you need to create k hash functions.

* `add(string)`: add a string into bloom filter.

* `contains(string)`: Check a string whether exists in bloom filter.

Example: 

CountingBloomFilter(3)

add("lint")

add("code")

contains("lint") 

remove("lint")

contains("lint") 

Output: 
[true,false]



```python
class StandardBloomFilter:
    """
    @param: k: An integer
    """
    def __init__(self, k):
        self.capacity = 10000 
        self.hash_func_num = k 
        self.bitset = [False for i in range(self.capacity)] 
    

    """
    @param: word: A string
    @return: nothing
    """
    def add(self, word):
        for i in range(self.hash_func_num):
            location = self.hash_func(word, i * 2 + 3, self.capacity)
            print('location', location)
            self.bitset[location] = True
        print(self.bitset)
            

    """
    @param: word: A string
    @return: True if contains word
    """
    def contains(self, word):
        for i in range(self.hash_func_num):
            location = self.hash_func(word, i * 2 + 3, self.capacity)
            if self.bitset[location] is False: 
                return False 
        return True
        
    def hash_func(self, string, bitsize, hashsize):
        hashcode = 0 
        for char in string:
            hashcode = (hashcode * bitsize + ord(char)) % hashsize
        return hashcode
            

```

555 - Counting Bloom Filter

Implement a counting bloom filter. Support the following method:

`add(string)`: Add a string into bloom filter.

`contains(string)`: Check a string whether exists in bloom filter.

`remove(string)`: Remove a string from bloom filter.

`count(string)`: count the number a appearances of a string, return zero if not exists.


Example: 

Input:

CountingBloomFilter(3)

add("lint")

add("code")

contains("lint") 

remove("lint")

contains("lint") 

Output: 
[true,false]




```python
class CountingBloomFilter:
    """
    @param: k: An integer
    """
    def __init__(self, k):
        self.hash_func_num = k 
        self.capacity = 20000  # set the capacity of initial array as big as possible to avoid false positive
        self.bitset = [0 for i in range(self.capacity)]

    """
    @param: word: A string
    @return: nothing
    """
    def add(self, word):
        locs = self.find_loc(word)
        for loc in locs:
            self.bitset[loc] += 1 
#             print(self.bitset[loc])
        

    """
    @param: word: A string
    @return: nothing
    """
    def remove(self, word):
        locs = self.find_loc(word)
        for loc in locs:
            self.bitset[loc] -= 1
#             print(self.bitset[loc])
  
        

    """
    @param: word: A string
    @return: True if contains word
    """
    def contains(self, word):
        locs = self.find_loc(word)
        for loc in locs:
            if self.bitset[loc] == 0:
                return False 
        return True
        
        
    def count(self, word): 
        locs = self.find_loc(word)
        return min([self.bitset[loc] for loc in locs ])
        
        
    def hash_func(self, word, bitsize, hashsize):
        hashcode = 0 
        for char in word:
            hashcode = (hashcode * bitsize + ord(char)) % hashsize 
            
        return hashcode 
    
    def find_loc(self, word):
        loc = []
        for i in range(self.hash_func_num): 
            loc.append(self.hash_func(word, i * 2 + 3, self.capacity))
        return loc 

```


```python
p = CountingBloomFilter(3)
p.add("lint")
p.add("code")
p.contains("lint") 
p.remove("lint")
p.contains("lint") 
p.add('code')
p.add('code')
p.count('code')
```




    3



#### Reservoir sampling 


```python
# Python 1: use when array can be fitted in memory 
import numpy as np 
def reservoir_sampling(k, array):
    # first choose the first k elements 
    sample = array[0:k]
    
    # for i >=k, check if the next value can be used to replace the elements in the current sample 
    for i in range(k, len(array)):
        j = np.random.randint(0, i)
        if j < k:
            sample[j] = array[i]
    return sample


reservoir_sampling(5, list(range(2000)))
```




    [1193, 867, 459, 1179, 445]




```python
# Python 2: use generator 
import numpy as np 
def reservoir_sampling(size):
    i, sample = 0, []
    while True:
        item = yield i, sample
        
        i += 1
        k = np.random.randint(0, i)
        if len(sample) < size:
            sample.append(item)
        elif k < size:
            sample[k] = item
            
reservoir = reservoir_sampling(5)
next(reservoir)
for i in range(1000):
    k, sample = reservoir.send(i)
    if k % 100 == 0:
        print(k, sample)
```

    100 [20, 98, 5, 69, 78]
    200 [109, 186, 160, 172, 78]
    300 [109, 186, 284, 172, 78]
    400 [109, 186, 284, 172, 379]
    500 [109, 186, 284, 172, 446]
    600 [109, 186, 284, 172, 446]
    700 [698, 186, 284, 172, 446]
    800 [698, 186, 284, 172, 446]
    900 [698, 186, 284, 172, 446]
    1000 [698, 186, 284, 172, 446]



```python
# use of yield: Yield is a keyword that is used like return, except the function will return a generator. 
# https://pythontips.com/2013/09/29/the-python-yield-keyword-explained/
def createGenerator():
    mylist = range(3)
    for i in mylist:
        yield i*i

mygenerator = createGenerator() # create a generator
print(mygenerator) # mygenerator is an object!
for i in mygenerator:
     print(i)
```

    <generator object createGenerator at 0x7fd96af8ba40>
    0
    1
    4

