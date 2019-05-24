
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
        print(hash_table)   
        
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
Solution().topKFrequentWords(["fayw","hb","yxbq","yw","bcvy","qin","tn","vw","whnp","bbq","icbb","yv","jp","cxv","bg","wm","gzc","jqzh","tg","cq","mqc","pa","mk","bypk","wipf","mfc","mvqi","njc","nhhb","dpt","wpd","cy","bzqw","bqbh","gy","hp","gyx","hqw","pkhw","bdac","bi","bj","wt","ij","mh","jb","fkh","af","tfqd","nyq","cb","fwc","mbtw","kn","gwzi","mj","abpb","wb","xwh","mhyw","gfa","izwk","cym","pz","hj","wqgw","dgmy","hwh","hbpb","gx","pxc","hnya","afbc","qxhc","cjba","cfva","dbqp","cnm","hj","pkbw","yw","mak","cina","zcd","cd","aytj","cxfn","tv","hf","nyh","kbw","ckx","wa","bbt","if","jcwn","qw","wx","cdw","qpbv","bb","hz","dxbt","yv","gb","xhyv","jyc","qbx","whj","cchd","dkqn","agxn","chj","ktfb","jvg","wg","qbyg","phjk","jzp","cfhd","pq","qcb","pkwq","dh","bwy","hdyq","ctgm","fh","hwc","tcq","acig","jqhn","tb","batd","ht","vkb","jwdf","wgb","chd","xc","mqdz","fjia","wht","bfw","bjyb","kyph","dbbz","wcac","bb","fcim","yzyw","dmhb","dmiw","wti","ibb","dcw","vdzw","bpbj","wpwc","zbaf","fiv","pma","hwy","yq","xb","zjbg","ac","wvb","fvac","zq","ja","fxjd","zih","jzp","qjwt","gb","wp","gvfp","cgyc","pz","tw","bizq","my","mwt","zn","jxw","wwik","hb","bdgh","mtk","jvwq","bh","hjv","zqj","wq","zjca","hchw","kc","yamy","yv","bmg","ygkf","an","acjx","ic","hgf","yhcz","zy","bqhb","hww","dyb","hpvc","nzwv","bgbq","bhbb","ww","yg","mbbi","baic","nvt","nbc","axbi","jbap","gdw","cwcy","bx","nc","qm","yc","zdi","gxp","bnj","pvq","pn","qb","hiwc","pdbc","hdzy","ch","yb","bwk","qbj","bamj","wynh","bjh","vw","ip","xay","xdcq","bb","axzt","jyq","bvx","nhty","xt","bh","awx","zv","tabi","ywiq","wyqh","ch","jxnz","in","bxtw","bw","acpw","gtb","bhw","win","bc","yhng","dywh","wygc","mgcw","ywk","avhj","ihvb","dgpi","bwta","hct","tmw","byd","hd","cxd","gxhc","qngb","hfa","fby","whfq","twc","gx","igb","mc","atm","khw","wk","nx","cy","wm","yv","iv","kd","phh","ib","ky","fh","ixfj","nv","bb","qp","fpw","wgk","htbj","ybab","ia","dkwz","nvh","wcm","yw","bfj","kb","dpw","dc","bwxh","th","fi","hbxd","wak","nt","zwqv","yvfg","xdb","fz","cmij","yjx","yc","gt","zat","zwfy","bv","by","pz","wy","fb","jhq","bwtk","whdh","vt","pqdz","nbji","fxb","ja","tb","xkqy","qbgj","dmf","td","wh","cj","bxyh","kn","gw","xz","bd","avd","in","pa","bdby","zga","jykh","cqkh","bp","yz","wdn","nq","xfqk","zbk","wz","hpc","ibng","nc","bcxd","yb","yb","dwp","yvak","fwh","bfac","ch","vfjc","jxci","bj","vb","mwpn","xfc","wab","hbwq","cyh","vhhy","yhw","mh","zdfc","ih","bh","hyq","bvcb","mfcd","gb","fcpz","czx","bqbz","ybjm","ji","mbd","wyca","pghc","yw","bt","bh","vw","qz","bbv","bcg","xc","ic","wf","jqmg","xbf","dkw","cqjc","wzij","hwck","wf","znat","xg","hw","xchc","bg","btda","yvp","baji","pjbw","hkdg","zcb","ccb","bmhp","bcng","gzt","tb","nw","py","vt","yyfk","ft","cwbi","kc","cq","mfxq","hch","wk","kmh","wbh","pkw","wgwa","vbw","cfd","cid","dbn","iw","hc","tc","afmv","mhay","zhb","bawk","camh","pb","tfcj","wcb","ica","cdjb","xhb","ia","nwx","iy","vgmh","wwcc","pyy","bxbd","bpyd","pgb","yixa","kwn","dby","gyda","jdt","mz","vcpf","bq","qb","zd","nzf","wtj","it","bmy","hbc","ca","gtmy","qw","bgxf","pqzv","chtf","mc","wkfp","ihk","wq","zgmk","gdbb","mq","gc","wy","by","ftgz","hym","gwy","ywak","ymd","jnf","hcc","yy","acyz","zka","bnfk","cgvm","qakd","bdvc","db","cxg","bc","wcp","tn","wcg","gypz","nkhv","wg","fm","tc","zc","db","dwbz","vy","ikb","bqdm","bdbh","vwip","wmf","km","dkca","wd","dkcy","zwh","hvd","pnbz","wdc","mqn","ygw","wqn","vwkc","fw","bxw","bfdv","yjft","nq","jbcw","gbiy","vdp","yjq","amdk","jbw","jmi","fm","ikq","aht","abh","dbgv","gapv","jv","ign","thn","apc","qcym","bnw","ccf","jv","bbj","fy","yn","ahv","mvtf","gm","qv","tbad","hya","zfhn","izyt","thy","fy","xhhp","hb","vqhn","iy","twbk","bwhh","qmtw","bzx","jb","bf","jhmk","myz","bcy","bc","hykg","bkd","ji","vwtj","bf","cvxi","nx","pyyn","zqyb","xzbb","hy","mnx","daz","fgkv","wg","tmj","xqwc","ycvq","hqb","bbfp","phw","ynyz","pbdh","gcbn","ab","yy","cbb","cgqi","wgc","vmwz","gpm","ghy","hy","qbc","chap","bqh","yyft","atqj","fww","jc","tcb","jiq","mhf","fcc","nwvp","gq","xqat","cfv","hgmx","igph","finj","ywp","cf","qab","jd","gp","pm","dy","ky","mjy","hn","zn","hbny","wqb","ybkf","pfx","xbap","yyw","at","cb","bh","ycqz","wh","qcyk","cyw","yjch","cy","pn","gbhz","zght","jdcv","qnc","nhbi","wb","chp","nhv","fx","gp","th","thi","iy","jhnh","fn","vba","xgym","bxy","hib","bh","apj","bw","yxvj","mi","nb","gyh","cyh","yqpj","jpwb","ncwh","bpjz","tpy","mnp","aytg","gxh","yz","dby","xzj","qmt","ykg","qnwf","nc","kbw","wcyh","bf","nh","jnw","bjz","fz","ta","yjy","tq","pv","zjkv","wh","dytj","tifz","vwz","btc","xwcc","bnxc","wbt","gbbn","bj","cbxh","bnjy","bpkc","qh","vw","gv","kp","gmqw","fqtg","wj","vpmh","yg","hyag","chwz","jhdk","kfhw","gx","qah","cthb","qihh","nbk","zydh","nmb","kw","dp","wjv","jd","ytxh","ivqt","za","iwcc","fy","cwwp","ic","yk","wxfa","ykq","qbm","qzy","hc","bbhy","bjfc","ybp","thv","bvc","kcxb","dhzw","yxj","kgwz","xhi","yqpz","hzq","zyby","xan","bq","waky","cy","qt","pkz","ncvz","yp","tpb","ahf","cn","yhc","fb","wfnv","ib","dab","yiqj","wqjw","jafx","jn","qipj","bfa","pv","hiwk","wab","mw","gpx","kj","qn","pqv","bfk","kayg","bwza","wbbw","bhk","pwhi","bdym","jyht","mwaf","djt","wm","ph","zayh","wzvh","zkm","hzb","nbyd","hvyz","mqfc","pbbq","nqtc","fv","jda","hb","ywnb","hwm","kfhg","wvhf","gxny","fmwk","mwcz","gxw","ztax","cby","fb","wyq","pzbb","vmt","bi","tb","jtmg","txc","cpw","bypk","dy","khq","yj","dqb","mgb","xa","zh","jf","afvq","tb","phbm","cw","yjwa","by","yj","qxdp","ty","qd","byxt","gmk","icn","yh","atyy","jy","pi","wgac","bhp","gvn","cwg","qngt","jvwk","nm","fjb","hft","fbbn","xytn","hmj","tcc","bwib","xyvw","vb","hj","wzf","fcmg","ty","kn","zy","chb","bhb","hjvx","hfb","bawy","pywt","bfbq","hw","kn","txcw","qb","yj","zyd","yawk","bq","fpnk","bk","qzd","qftb","bm","qny","dxv","bni","cmb","phfh","mhp","jp","yi","hgim","bpd","ck","hb","bq","bac","avfp","kb","yjw","xbq","btyi","hxb","jh","whqc","btc","nyja","qbcn","wzcb","yh","hih","nc","mth","ybpz","hid","njb","wtq","yjmh","bb","jt","dth","hnbc","ydpb","hib","nbjh","hnx","cajf","kbx","qxm","bh","xvbj","hw","hnj","xiq","jyq","vcc","cwpt","fd","gb","jqx","vz","wqz","ygh","fidw","pcdm","nbma","mp","wav","htbw","paz","xc","pxi","cd","gkw","fq","hk","hdy","wb","hxtg","th","khdw","dxv","vfqk","yw","mwcd","hnwx","qci","kw","wta","hcyy","cdn","my","wzj","bcgb","wi","za","byd","da","bb","wmkf","mxq","wp","xkt","fhvc","ciw","ykm","ka","bbdy","bc","bzxf","jbw","jfc","mbc","bh","cq","yby","nbzh","vc","fj","jb","iza","gx","icmc","xwn","tcfn","ngph","mqcy","ygqz","wnv","hbm","tymh","ghzp","ckf","jh","mxay","pbya","qnhd","bhw","cvzp","bg","igw","ykcz","hnpf","yhqc","cdig","tx","bbf","wcf","vw","bqp","hyt","xtb","nwja","fdbv","diay","ix","wdg","wbmt","itx","maxh","xcbg","yciw","icv","bfq","gjwf","kw","wfb","hqp","mxi","wy","vb","npx","hk","hxy","zbv","qybj","vw","wgxa","cg","pk","xa","zbkq","mt","dmw","dx","gt","fbx","ck","bwpx","ybwv","jx","bhtj","iaj","if","bkwf","wbg","wa","bcf","xj","gbz","tj","wyb","cb","wtmc","hhc","zm","ih","bcw","bc","bfp","nkc","cwc","vh","bcy","bynh","bh","bhpm","zchx","ibvx","kicc","ckbg","zkht","tchc","byq","acx","ctp","bznm","hhbw","jhgd","gb","mhwn","bcd","jct","gwvb","gbh","yh","xwkz","bzmw","zpt","bkwb","cp","jhpb","ybq","jy","xjhy","qaby","fga","cajm","xghk","bz","hgw","myq","dnxt","kqww","qg","fw","gczp","dgyw","wn","bkb","acmy","ihz","bgmh","yc","nhhq","mvta","fvbw","twqn","cxtw","ajw","wyqh","yavd","xbyt","bn","aqc","wbda","chk","ywaw","cb","wpz","kxz","agib","hd","nhwf","wya","bccq","wbi","dax","gc","cw","my","ccvy","wgkf","dcbb","cbmp","fbpm","kj","qwcy","mk","bvct","cch","dq","bwim","wdy","cxmk","bv","ca","bh","bwwz","wix","wq","nqwb","niyp","tjv","mdxp","hn","gy","fznb","ybz","fdz","tv","hbt","cm","fnjw","nbp","ack","vbw","cjci","cf","qy","vam","wht","ixm","fgcc","im","hbx","mbng","vtwh","hfza","piwf","pwh","ympb","bc","ibz","zkv","bka","pcch","yf","xqc","abzg","aymk","aby","hym","bk","whdq","bfy","pwyb","ch","gab","kvw","gx","hfgv","mbkh","wch","bgk","pwg","jf","xmw","ywc","tghw","pw","dnwa","hbav","nip","zmgw","dz","kcvg","bpx","mic","panb","iz","vfbj","ah","cpv","bidx","ibyh","bhn","hckt","ph","hw","bdz","fbbq","bpk","hdw","nt","bw","ztb","yxw","zp","ytj","qj","vyb","bti","aky","pqd","wxgq","bbkb","wqi","wbdg","wt","bbdy","gncw","bycm","gh","wyyt","ijc","bh","nax","bb","hqx","fgy","yxh","jhh","cfi","hpc","hgb","txb","kd","cbgw","vbn","gd","hvaq","yjtd","dbct","vkbh","bd","kbn","faqh","ibjz","ntib","gy","dq","qk","kv","zhvt","byj","gafc","hcv","mqba","gnd","jiq","wy","qf","hzbc","chym","nq","bht","bywb","da","hfw","cfqv","bxc","zhj","jyc","gcw","ypz","kbyn","bhg","mc","yb","czkd","hj","wc","tz","vihy","yfhp","vgh","kj","xp","ijb","cn","iw","jwic","abzb","chb","xk","zkx","ybh","tayq","yw","gca","jby","pb","bkc","bbyf","qi","nhyw","yh","mcy","hwkx","wygc","gx","wbxf","hq","jban","mdtj","mi","ag","xcd","wz","ybnw","hwy","ct","bfyz","xk","ckdj","pfkg","jfc","gwm","mbbg","fm","za","ahpm","yhjq","wnbv","bz","zk","cp","znf","pymh","qwh","jm","pyj","avhg","nmc","bhh","gz","qmz","jwc","bnt","ghby","zp","yb","dxby","yc","pgw","mxv","nj","qd","nmy","hq","nk","fb","mcfb","bi","whb","ydab","idp","adi","gbi","gw","hbvz","pbb","yc","wkgq","kxf","kbc","dx","by","kiw","zbtw","wfd","ytd","vimn","kxz","ktn","tw","ychc","hm","xz","zwy","dq","hkx","gw","nbjw","yib","ndp","hbfp","cy","wnc","gj","dh","yq","gwfj","fmqd","bn","yxnj","ciwy","cbc","ch","gt","dxw","hjm","wh","wh","kbph","fdq","hjhm","ydam","vxb","gykb","dhj","ax","ck","xhiq","gty","wyjn","abw","whfv","qjnf","xycd","ycm","fdk","mf","wy","mnxd","nhwb","nbbx","bmv","khjx","yb","kf","vw","iy","yjg","znyy","yzi","iwzh","bijc","bhf","yt","yv","thdz","ki","avy","bw","vt","mtz","yndb","ck","avdp","fb","ydfj","dmzy","tb","xb","wcnc","cb","gjtw","cc","dvn","wj","abti","wp","bbi","qzhv","yfh","bfy","cz","bp","zg","bwd","jnmb","th","bhy","bic","yi","fhmk","xzy","hz","mcqy","gh","ngj","hcfz","cb","ahvx","ajgv","jw","bmk","tb","hh","hcbc","nqw","bpib","dvjz","xqbv","bg","bfz","gzcb","nbzx","qtv","ny","byi","wb","cmg","hi","hbh","ahdw","imw","vyfh","pxhi","bh","ytfk","bdf","hznb","cnq","qhcv","vwkg","yfbw","hcd","hb","wy","bw","dym","tanj","ba","czwy","jh","pwqk","cq","yqfi","htw","fat","pdz","bcbx","cww","cgz","bbky","zck","bbb","qza","bw","yjyd","icf","hyhw","hv","pchi","ym","mwfc","jc","byac","jhdk","yz","wth","ykf","kx","cgti","aww","ydbh","mnzw","zk","cd","bxy","atm","cbx","pfxh","fm","qc","bdzk","bgca","tmbb","kbwb","tm","qhf","byq","gh","xbh","gd","zqc","jthh","yg","hxpc","ytji","xbbk","tn","vk","gm","xzt","xc","vfh","fphd","bwfk","vzxb","qc","by","hwx","vj","jxhg","dn","qd","htm","bdv","jb","ijbx","mvz","qbch","ymw","ziy","kja","jxbw","banf","vb","byf","kb","dbk","ib","qm","zbn","vty","nmvk","by","nwhg","gm","jyd","bz","wkt","fg","xbv","pytw","acbb","cbvi","cy","ha","ikxh","cbw","hin","ckb","hy","pycb","bbwz","ghh","xcq","dn","whnh","kmy","bdw","cxy","qyip","wz","tbc","ytmn","dm","cnp","wi","gc","pcb","xhia","bjc","myvh","tn","bm","dzm","nc","bpky","yfgm","bgcy","fam","jh","wny","hhmj","bigw","fa","nqgy","ph","ix","bc","zh","mi","zdb","ydh","xw","ymf","mj","cbw","cakj","ynwc","tn","xk","atwv","zabi","hmq","ty","nbcb","yhba","wqj","nag","mxhk","ptj","cbfv","vcb","thvy","ncb","tkj","gcz","ay","hcv","hk","hd","tgqy","vth","bm","gwc","bvjy","wcg","fqk","nag","hy","yd","cznp","vhpw","ymq","wbi","pdch","xzy","ycf","iw","idma","dkij","nqbc","jhzp","hcma","wc","ig","bhn","wc","wywa","atpj","jf","fc","ctp","wcf","mn","qc","wkp","ckft","zcyn","yxp","hbv","hxfk","hwpm","fbhv","nbyb","wacx","bh","ij","cgyt","kb","xc","jax","bbn","cqt","hwkm","tyb","qbxw","bzc","hbk","af","tmb","nj","bn","ywiy","ht","wzb","hc","ybhm","bhx","cmit","wk","itfc","jpd","qbyd","zb","bby","ab","jz","kvxw","kq","in","nbq","bphv","xb","byb","kci","jd","kpq","vbw","ah","ybpw","yww","ba","mbf","qwy","afj","cmby","byaf","jzc","jp","znth","gm","cd","xchy","tcc","pib","xygt","cg","nf","qbbh","kwz","achb","byx","mf","kvhd","dmk","bdp","yc","pwi","jb","yk","cd","mbc","xt","ygi","hhc","bmtg","ynbg","yxi","fpw","bk","qy","ijqp","ig","gbdv","kmby","wc","bzf","acfi","hq","phww","dycz","wbqz","cw","whjd","hf","gw","jv","ymqb","hybc","tbcq","nd","bx","hw","hq","pvf","yhx","kha","cz","qhw","jax","dvch","czh","yaz","my","zcfy","wft","nv","bk","izh","bmx","mt","bjc","aq","qw","xn","dch","tp","wnc","cbq","kjb","hcyi","cp","cy","bfy","cv","hbf","tz","kyg","wh","bfqy","zcc","iyz","yc","hikp","xb","qhzb","fd","cbz","ky","jn","ycf","na","cfjw","xmk","htvw","zp","jdwa","bgy","awwx","wy","wh","wzmn","ybzn","yd","ga","ky","ybpn","acth","bnj","nb","jwy","pvhc","ja","qfb","qtf","xmd","jcw","qp","qwiw","bj","acgz","cw","cy","qfcm","xqf","jc","fy","xyby","wzq","btwc","bny","idw","bab","bay","bh","ty","fh","aw","byp","pchz","bt","hm","xqzc","wbdc","mbb","gjtn","ww","bb","cxd","wbdf","wz","gyc","dbbj","wx","gbm","yzb","kmbc","ybxp","hi","fbp","iv","qy","wc","yy","zhb","cfhx","hj","jkwh","vg","wyk","hk","yxi","kjhm","izm","zwha","ihcp","hkgc","hab","bb","jydp","jyp","bmn","biwn","hk","xqbp","wb","npg","mwpb","ibz","qfcb","ywfq","xih","wynh","fyc","cit","ycb","cg","wct","wjyc","jm","wkh","qtgc","zk","qxgp","mwf","za","xmqt","ivyt","pb","pdcb","zdf","kmbf","va","zv","yw","vh","cwj","vb","qxy","jzc","iy","iy","bch","fkyg","tg","fz","hwcf","cam","xvd","ga","hcg","hw","yw","btcd","bw","pm","dgci","yg","wyk","qmkh","xq","gk","gi","yfzw","bt","wth","wcy","gbn","nwv","cngw","inp","cd","fjpt","hbty","yc","ch","yvph","wf","hj","ihya","kgcq","dg","yik","yzdx","wy","axk","bw","wx","wg","tbx","jfwm","pfj","tg","izfm","tcw","wi","hawq","kv","pgxw","yq","wb","gan","ty","zhyg","yzcb","tc","bcgf","pwb","fdx","wz","hgqz","cq","ty","fzmk","bw","wnx","qbyb","tnph","hd","ij","dgm","mkh","xb","xhk","gbc","ygwh","cyy","bth","aqb","pmh","gbc","hm","ntz","tj","za","fc","zwy","xw","yh","bhh","bba","hbv","fa","qc","mhx","in","biq","fmw","vhb","pkjh","tbx","ma","vha","dbx","jyvd","pcv","vc","bhch","dx","vnwz","dbb","mtd","hpqd","btbw","np","pva","bm","xcdy","ngx","baxt","wqb","hbfc","zm","gyb","hvq","mgwv","nmz","zygd","cf","hyby","cz","hz","hdw","zwnm","jywc","ywyc","bwhw","hpbx","wyq","mdhy","yybb","bw","qtf","yv","mbdi","fhn","adcy","gwpt","hxtw","yw","qbxj","bna","cxp","bk","xhz","pcz","yjhb","bzip","qm","tv","byv","ga","vpda","gyz","wkg","kid","wxa","ikdb","pqmf","yfg","gbqw","vb","xy","db","bhn","ytww","bn","ybbz","bqbx","hnc","bvyp","kbyv","pjn","di","ph","ynxb","qbd","hwjv","qb","wdny","hgxv","fi","bcw","aw","yc","gh","hcx","yxbh","mg","vbt","bgkc","jd","hwga","ndh","bz","wcv","vhcc","kw","vh","wvyz","gp","wh","zx","tn","wqf","cjgw","kbt","py","zc","mip","wftq","byx","ciy","hwfi","wqmv","cch","gtv","gyjy","vtqi","yp","cb","kx","wb","ha","ckyb","mccn","hp","vygh","thpk","mac","bfx","wb","wwv","cphn","pky","wbf","bmcz","yxf","tmf","fbc","ybj","micc","bnxf","wz","mfgq","ab","hy","hb","gb","hyib","bhc","bh","ikcz","vbbx","qy","vma","acdn","zwqy","yv","bdz","tgw","kv","cygb","wbp","bmxh","dc","fymp","wn","mpb","dvcb","fyn","ixm","hnz","wmp","bzy","bbw","pnhb","mbjb","mwxh","tf","tbk","byi","bif","jnhi","mnpa","htk","gmbv","igch","ya","mhch","fpb","yych","bv","ncgx","yncm","dka","jhky","chz","bf","zdwc","qwic","pibc","yy","cxc","cgby","nwyc","ft","cpmd","bqdi","qb","fy","ywi","avmh","qhy","tbbw","tx","qybd","fbgc","dg","bcwf","qbh","pcy","pfv","tvi","qk","hz","wpa","wd","jfdt","fa","ha","yvi","yxg","nhgj","ict","atwb","qmi","cmbi","jma","hmhf","tb","zh","yc","qi","wwc","vd","xvpi","fz","bn","ythw","bay","zfd","gwhf","yb","ayb","qdba","pix","yc","whx","fcg","tqab","iybk","hw","cn","knc","htyy","dhc","jybg","nwjz","zbj","fjvn","adbm","fxny","ndb","bxb","vwpz","jva","hm","kqwn","mdbw","cwy","btp","hqif","wbnm","mzk","ayvg","hq","njx","txw","jbh","fnpx","wth","zbkb","yzh","hv","gky","ccak","wy","cvt","phfz","yjxq","hy","id","bbhy","vq","hf","ibby","pcqy","wj","bf","mhyh","cacn","wt","iy","mpiz","ntap","ahq","zqi","wyb","cq","bbtw","hy","bj","zh","cqdj","nt","yiqb","acxj","mt","vjix","byfa","nhd","tnzd","hwcb","pdm","ihvd","wd","xwdb","fhy","ix","ct","nwkh","pyt","xdk","ydcq","hxhb","bqwb","tcg","hy","khi","bixy","fyqb","wv","hpbx","qhbd","hqbt","ny","cnvy","cqbd","tcwi","pfwy","qpg","gcxt","bc","ygy","wg","wjf","ngvz","ydi","avhm","qy","qxt","axni","kyfq","mwb","dh","gh","zyww","hbjm","kz","ay","fqy","dh","bjf","az","py","qmxw","vb","ykw","fbw","nv","zt","vn","yjx","haid","cbh","gc","ybx","btbx","by","izgj","mbca","ky","pmbh","fdv","hp","wbav","pz","bw","vakc","jx","qm","yd","kcmy","byby","wb","gb","vxm","qk","vbwb","zhhx","fyt","yvwk","pz","ytv","iv","bxzd","cpdb","qyg","byjw","zxw","hmjh","fyp","mxcz","hhb","xip","tnyb","ca","hwvd","kwg","bjv","ybbv","ptv","gy","gxd","px","fbnc","yq","qm","fzb","xwk","qhb","hh","fjpw","wx","qmcb","yw","fc","zc","ixpb","ncwx","yq","xm","cf","vyyh","ncb","ycbd","zg","pb","gyhp","xmb","yn","cz","vic","btc","ix","gd","fcb","bbx","gw","waih","kiaj","kmgv","bf","cydw","gxpc","hj","bby","jbzh","bnvf","zcw","nbhp","txvz","bc","tbzf","bw","jb","ga","djwa","hqxd","gfnw","zy","tq","jdvh","vb","ky","bhti","hqb","ym","qp","gbvf","wkg","zw","fgqz","hiqg","im","zdmp","dyq","fjc","mwyj","zkg","xb","gvbd","fb","px","nb","gfmk","ygyw","yf","nxfc","yq","qg","pk","ywxb","qk","nv","phfc","gj","tc","pwc","fk","dn","hi","cb","gbb","kyc","cnhp","ch","py","awmp","fb","piw","aq","bv","dthx","mwx","yxbb","tqac","cnb","ta","cbm","adk","hwnz","mh","qi","gvt","xkb","htm","cy","zbbv","pc","ibn","nqi","yqy","vag","bmk","qdhc","yv","tz","wjzb","zgw","ib","kip","jhc","hfj","gaz","ynhd","aivw","zx","ww","px","bgch","bwp","jnq","gzpa","ynk","fybq","hy","gdy","dp","jxh","bf","qcm","wc","zb","fc","vhdp","dinp","wm","bz","gvz","ik","chzv","hmi","gjpi","ybfw","manj","dw","tyd","xww","tfq","yya","ywtb","ikbb","hh","wq","jw","yj","vbbc","xmbk","dbhv","yb","ib","pm","bfh","bkv","bx","imbc","jt","bdf","bw","nmqv","hf","xcw","tiay","yb","njtq","kczy","ika","vfh","wid","wa","bzy","hb","pyb","dbci","wyzq","ctpn","zqy","tgn","mxc","whhn","bghv","db","btd","ki","nmxp","hc","tw","fxqh","wc","wcy","tb","yw","gvk","ywqg","bz","bbch","vcm","fbhw","bbzw","gcfw","mh","wg","by","mtih","ity","dh","yx","wcb","txh","dbqx","bnhc","vhb","anqp","dq","fq","kh","bxnb","kfy","cijh","zc","xhba","pg","hcmj","xcj","bhxa","gbv","yg","mb","ztvd","kyh","ft","hqb","fwvh","kc","qi","ah","zh","gbyi","hip","wy","jzw","jat","hy","zcmy","mnw","hbp","xm","ibdn","vdb","ihjc","wmfg","ya","bv","iqc","whc","ky","tzw","mbb","igd","yhcw","mpc","twk","dz","jh","gbc","ty","wx","dbv","hgj","cyb","xt","wknf","ci","cd","tca","tyc","ni","ptq","cyp","dbqp","bwz","bb","mtz","kyqa","wy","fcj","wdkz","pc","qwy","ybwj","hpxb","dj","by","ww","nvb","wxyw","vzt","qybw","wkf","zj","mt","iwhy","xgw","tyw","fmcd","qb","tnx","fzbn","ckbb","dwkh","phv","af","by","qpw","kq","pc","jwhb","fd","qi","ip","kcd","bgnf","pgbm","wmh","dbi","yba","nq","cn","hmb","ga","hah","jy","pmi","qkma","ach","cwd","wqmg","tn","gwc","ay","qaph","vky","mw","ayb","xwm","tjh","jdbi","pwwt","btf","wmty","gnfq","zfc","nix","mhb","jmwq","pywy","ifh","wbfw","qtyi","ybf","pmcn","myw","ab","hf","vxmc","bcyn","wa","ic","kzqc","bci","fhc","dy","fy","fyd","bg","zhjb","wybh","jmgb","bx","pxi","aqc","wbf","cyfp","qh","nv","acqv","pbkn","fyv","xt","fcn","yfgw","bz","dp","wb","wpy","mchc","gaj","jahh","mfhz","zfi","qg","py","hgxz","mxy","fcig","pchh","ynmz","bp","ydf","hka","bx","bfb","tb","pkb","ygwb","pnwf","ny","gyp","bwbm","fnqj","hbcg","fa","bwx","yi","khhv","wcb","bt","ig","hzb","cqk","xybw","zxcy","fj","pkgw","wgi","wwdc","mx","kd","xit","bbwb","hm","vbw","bfk","wa","xkbi","bqc","cd","my","wpb","fm","fc","yhxg","jg","gw","zbd","zdbh","yh","xcbj","wax","ncb","xph","wcyb","qbgi","kym","pf","zk","bncc","bqhk","pcf","cb","zbiw","cwax","xtna","iyw","gjy","af","mgt","cj","xfj","mn","ymgw","gb","cngf","abqm","yn","cf","hzx","yncx","xqvi","ycm","zpyh","ygf","chbz","dbbg","cqk","bwv","df","cz","np","wdn","mwn","zyy","kww","dp","ywq","xpn","dgnt","hy","yzmd","jyha","bbnw","ihzm","avh","qa","xb","bbwh","av","bc","bi","qvh","tdn","icyz","bdh","xqzf","ybxy","aqb","xpty","nb","hbbc","cyn","jygd","gib","wyi","inqy","gqdc","gh","tx","wp","dtc","pw","mzb","ctb","apq","bthn","zdyg","zp","cx","bw","ac","wn","ibx","xzb","ib","qbhf","phk","fa","bwa","hgcw","wg","ac","tpnh","abpg","vbh","zfv","ngh","aqy","wdan","yq","wgh","ka","pz","nv","ipk","advw","mnid","yh","cbd","ig","cww","cw","pfhd","fxkh","nx","twyc","ybmq","cxn","kiqy","wb","vt","ha","cpjf","kdz","td","ibxt","nyb","bxny","bhnb","jbw","yh","zcb","gdy","hihx","ift","db","qwt","fmdb","mb","ty","ctwx","ym","jd","ymjd","myb","an","vc","tcb","wafb","yb","fdb","jm","hcwk","hm","wcdn","mhz","fmvx","wqkg","ichy","jkv","hpd","yif","cnxh","px","wq","ycic","mnw","bzj","xkya","wbhi","chn","txyb","tchk","kzc","cwtb","bq","ib","yik","cc","hkb","wy","znw","xp","xnb","pic","wabc","gc","df","ncbh","cqj","wp","yv","jbqk","jvpb","fih","dba","qv","wccj","wavw","qcc","gzv","fv","yhtb","fcby","xckc","nbb","cthn","qvfx","gm","bh","aw","bj","zhw","thfb","bjc","hm","ccqy","iw","vpm","jqib","cbzq","zda","hy","xcnb","wb","qwg","jbtc","bdm","bq","kcb","ch","qb","py","yik","fckn","cc","yvhp","bw","ijmp","ahbd","gfz","fk","jb","cvx","awc","qyg","hvy","hph","zi","bvp","aif","wb","zgf","hybb","fhnd","km","xjqm","wyh","dmpt","xh","kypn","hbtx","wn","bzj","dvpb","chy","bphj","djc","bq","km","bx","wqbm","yzhy","bxh","kj","bc","aiyn","xz","yni","bbtv","tim","vnh","ivjb","by","zxcc","ihb","gcb","wb","xhq","tgc","hbt","yq","mjbf","hpfn","bywd","bk","wp","kdzt","iv","fjkb","kzp","yp","yi","thk","qfa","kw","cf","cik","bym","hhm","wcqz","bqhv","ybcn","tmj","th","fcg","hv","kgdy","vn","tbyw","yhgx","dyfb","mwk","pwac","nfip","tiyf","tvx","ikwx","bcvw","kfbv","ymy","yjwb","dhm","jy","bgzk","tyk","ht","hbgy","hwb","tb","agfb","ywfg","zb","qd","hc","wc","ym","zbqy","dnp","hcdf","davy","bhw","qaiy","aix","higf","iwfk","bhjw","ibbp","tvz","whh","gkpw","nhp","bc","yiyk","cfyi","yn","xyb","dnwp","wk","th","bwb","jmn","jbnb","bzkw","nh","ch","cf","nqy","hi","vx","iwfb","vbba","pkjc","bkb","pa","kvbd","hn","ji","mtp","wym","cavm","fbiq","iq","tqmb","khaq","cw","kzb","xb","hjw","bm","kynb","tagc","twgk","xfn","yfa","bw","bm","pwt","faw","cj","hmb","bgdw","ya","yqy","bbm","gfhy","dhwk","xcjp","nwaf","hdbb","ifyb","iv","xh","mg","md","itkf","kw","wbc","nhp","cbn","dwqb","gfc","cvbh","mwc","ckmv","kcx","cth","viqb","pnmy","bic","zw","fy","njv","vy","hihc","bj","ba","jqz","hfp","hwy","bg","dctw","qhwn","tc","gb","bcd","hyc","ibcw","xb","tc","pqbf","nb","yfic","bt","fcjt","diw","zqy","bbyz","xnd","djtb","wc","bcfa","fc","cp","giy","dqbc","cz","cbmy","hhzc","wkyb","ynav","iak","aht","atwm","jzw","vd","bmh","ifh","ji","cgyw","chyb","bc","fbh","qd","wpm","hicj","fkg","yhqz","bq","ybx","jhh","yt","qfxm","matz","hw","nc","yinc","cwxb","czkf","hi","kbw","wpz","ch","hcn","xb","kgi","bj","yn","hjt","mcty","aky","nxh","bijw","piw","kn","yqc","zb","whiv","yi","cbkw","xb","ykh","hcq","pykg","hbc","jbtz","pn","gkn","iht","xw","thp","tb","wxca","ia","mw","yz","xnc","ak","zq","pbvi","dmcg","fj","gxb","ywnh","caq","bhz","bjdq","bbda","vx","idyb","wqh","qbbm","hbm","hdf","ab","bgky","ccp","hdx","yngq","fj","wy","ym","fijp","bbz","ywv","ync","kw","pd","bbyn","bfk","qh","xc","fwh","fiv","gjmf","icc","cwqa","hdj","wc","mxwv","qvy","ag","dwq","zf","kby","qi","cyzw","bmcb","kbyb","kwcb","bhky","vkx","tk","wad","cybi","ybn","ct","hb","zjic","vpq","jd","ah","hjiq","mhw","jyt","mx","ad","dzxb","ckyh","zhp","jc","qj","iwb","wqmf","ckhz","pyw","ak","avyw","pkxg","yb","dwz","zbp","jydh","kbc","cg","azc","bmbc","kqj","cmjb","zxt","zp","xi","iq","dv","yi","jqcm","qj","wpjh","if","ybh","nhk","pw","xmd","xg","kx","bx","hna","bph","zhaq","dqyy","kbab","vmj","cvb","hpzy","qpbw","qb","xq","wdhh","bnxg","jnh","bza","gzn","ym","knb","wb","cwvb","cjy","yz","cxhf","nf","yhb","nwg","byb","bft","bpth","wvn","zfb","cwpb","jcbw","cb","tbf","nzb","nay","vt","cz","qndt","xva","fc","kb","wa","bcwx","wm","ykht","jhb","mzn","bv","bf","hwyz","hy","xv","whhy","ijxc","hwv","vha","bchw","bjtc","wgxd","iyb","vg","cbpb","bgph","chw","cgc","phw","nkwy","wqt","fbd","bc","bbc","hfd","higw","bfy","mh","ycgx","wcmn","bcb","bai","wk","dw","xhy","nwgh","mwab","cfbk","jp","fbwd","qywx","tbb","cv","izax","btp","bxw","cx","ka","ab","nj","zkc","njw","fqp","wb","bm","zt","cw","cbpk","yi","tkyb","twck","hpz","pj","dw","hcf","hy","giwy","yf","ghb","pmfh","vyb","wh","hcp","bz","xbam","kapm","dcfb","pw","vcat","pzbx","dc","wnx","gy","dgmc","cy","hx","awdh","fyp","dji","wdp","ytb","cbxc","nkx","wc","fgkd","wkj","hba","pcbh","vkn","fdm","yhm","ygbz","afnj","bwt","ciw","mbh","tnz","gkj","tk","hy","yyba","ywmx","fcwy","hch","apb","vxby","bix","viyw","vzw","fva","ak","nhmk","ynf","qfbi","akdi","jgtk","ywh","dn","dby","cd","zka","pnct","hnj","in","yzbb","xjp","bzc","qyn","gt","icyk","xfz","nyya","kw","whci","bhmn","hicz","iqg","bxic","tg","vip","xzd","ga","qihw","bd","xk","yhhz","cbn","apf","ag","jn","bhcf","gp","mczy","nyf","kc","yw","bb","wt","awz","iyc","jfi","pbc","dk","yyfk","ni","xh","mhz","bk","idg","wd","cqbj","nzcf","dgwc","bv","cwqf","vh","bx","jpq","hv","qhjb","ibc","pv","zwdv","xa","ym","bh","dyc","nm","hb","ayd","ynzb","pgcw","cvbd","ncch","fp","dbw","bwcn","wk","hbgq","zhf","hyf","jawg","cvb","yx","qh","gdx","jbh","qca","td","xz","twk","vb","yb","vk","bcy","ikbt","invh","viwh","cxb","hg","ahp","wv","dwc","awy","bwj","ywpn","fx","bg","hm","ahg","bj","wcbh","nfwb","cph","mn","bwy","bph","xt","qf","hzm","pkx","zn","qck","fvqm","ychw","nmh","nt","ytm","bdta","xh","jmhc","wc","zx","pv","gik","gq","miyx","wf","fqim","jx","xbj","wvh","nvm","bky","fnc","ct","bx","kjw","zywj","dhbi","pm","ypg","ay","wfb","wpy","anxt","hn","whfy","tdcq","thnx","bmhh","kd","mbtc","cma","cnb","dwm","wdbz","pbvn","jv","qhp","kqd","id","ybw","jk","dzw","cd","jvqw","zhvy","nwp","ytkw","cc","pykb","qzy","hy","hknm","ccw","pmy","zg","kb","akc","bqy","vj","gmyz","awbp","azq","ca","jcct","vyw","qh","hx","mytc","qvw","mxwj","mn","bmf","jwh","jchb","mgv","tnz","cba","mccx","ycn","fh","hjy","ab","wghq","bj","afb","fwhg","vwck","mf","wp","twhk","vxb","bt","vh","bwm","by","hpyx","jvky","jqp","qwb","hmj","fb","qjy","yah","kba","af","wp","pkwm","bi","mt","iqp","ph","fj","cigc","mpjq","pmn","qczn","jpi","fkhi","dbik","chf","cqx","pxdk","hcqf","jt","wx","amdk","bhzw","zfg","xjpw","vi","nkb","xy","ijwh","vzwh","zw","ybwp","tyw","fdzb","ay","tawh","cb","waw","qzc","wtk","kwc","mcxb","gfx","icf","jwh","dbbp","txhi","hg","pxb","my","zt","gd","ybhd","qibw","xy","cb","bch","yb","xd","ch","yqw","yp","ym","tn","hbn","bg","xjig","aywq","pxb","zw","kx","cykh","wci","twgc","gdnc","ih","hh","bf","ichb","hcny","wb","btbx","dx","kiyw","mx","bxp","chb","tc","pm","mw","vhb","xik","bhb","hah","xivt","tc","gqx","wy","tfhn","jqhi","dymw","jp","dn","fkam","mvd","iwy","cbdw","hd","pw","vcat","vtxg","mwc","jcnh","mbiw","jc","ycmg","wck","pvfy","xayw","za","nhb","yd","vfc","wncb","zpk","fb","ki","pgnf","mqch","wx","hx","bbk","cix","mzy","hbc","amb","bk","nxb","di","jz","fh","ytgz","mx","dyt","fnp","qw","jipf","vybh","fhi","ywjh","twgh","by","mc","wdnb","bc","fdh","ctf","qwcj","gbc","nwb","ka","tbw","yqvi","pw","dbt","av","xc","zdc","mfx","mckz","kqmg","yht","cpwn","tx","ji","bv","gq","mpfy","bwcz","nb","yn","gbkc","jf","fyjn","xb","cb","yt","awfw","mwgb","jw","wpih","gx","bwnq","bxdg","qvxn","yf","vkh","inh","gnvq","cyb","yb","htp","nh","avq","abty","cma","qwfy","ab","ybpk","pzxy","hqg","jnxw","kqwt","bxy","cqi","kva","ivj","wij","bzj","jy","xy","bwab","bpb","bq","yt","anj","wj","yn","ci","bni","hy","chx","cw","ya","gbqh","xgm","ig","fqiz","nip","ictj","dpq","wf","dyi","wyqk","pm","wjg","bycq","bfcp","yw","ayqf","nbgk","yp","zb","fdyb","ndxw","wcv","wb","hbgq","jy","chby","xgcy","mcjy","ngwq","bb","gbwd","dhb","dzb","qt","bfgy","kzx","hjb","wmj","ich","gyam","zvjt","af","gab","mc","wdv","wb","iwkm","ak","hb","pmq","tam","zwwf","jhg","bnw","yxc","qhbz","jxcw","btw","bhwc","hzp","cbg","wib","hbcb","xih","xgfy","yjic","wny","cyb","fn","cqfa","jcxb","bjwd","bj","gib","gvq","bnam","yjq","aq","zxg","bm","nafq","afhj","hh","tcxc","jtf","wzp","cfj","bjac","by","nb","fw","yq","ikwj","xbb","ky","xz","fb","dnyg","fwb","kp","ij","bydp","gbw","ai","bywf","tjbm","kj","wyj","cw","px","yb","hyh","bqt","wkc","qcjw","kn","bgk","ba","qcpz","bnc","xh","qm","tpx","za","wx","ccj","wqy","wy","an","bwf","cb","hn","wtb","vfa","qv","cyy","pccq","zqcp","qb","bxj","hhy","qwid","bk","pzcy","ncbc","hzc","khz","ckhc","kyib","jgyc","kjcn","bmz","bkxc","cjxw","in","fc","qhgk","zjn","gw","ww","yw","jpd","bwa","bgpj","byh","zh","bix","wvg","hxbk","bhv","jtd","zav","bbya","dyzb","zxt","wca","mbic","tg","qkab","gcix","ynw","bza","jbna","wbt","cabk","cbad","mwpk","bmjy","yag","mfq","cmx","tc","ity","gw","mc","fk","pcf","cz","qy","hbxw","dhtb","wk","igd","bbgk","hjwc","zhpj","typ","pqy","kwcj","bf","ixg","yvw","wbx","cww","cn","hwwy","pyhv","gz","ntfb","qmc","bzc","ifx","zy","in","fhyb","kv","bw","nkyb","cp","nqap","hm","fv","qcvz","gdwb","cntz","ptb","aymf","yn","bg","zx","qyw","dytc","yjbc","mcn","vk","wki","whh","xb","xwfj","wt","ib","bx","cfz","cbyb","ibm","nhb","wc","pbx","anhc","wpx","fphc","nbdk","cqhb","fpyc","fzy","bc","gc","zw","kzh","dwy","cctw","vwnb","cwnd","yq","nkyq","hkxw","jx","gc","tfwx","cdb","gk","mg","dc","ctix","jbk","vm","km","vkb","qbzh","jxw","afxg","bvc","fk","fw","pnc","yj","ny","yzdq","cwbm","wc","zqtj","dx","yvdn","iftm","pgb","tz","mwpv","gywm","zhw","apyb","babz","nf","kzd","cy","hh","ww","cm","mgk","bj","wbwp","wcg","zyyh","qtdx","byc","hvb","xw","jbah","ah","mbcw","jpc","hh","hcit","vt","vtb","htb","hj","zpc","dh","cmbj","yhxk","wibv","nc","ck","mph","tc","wnb","hwvg","cww","ja","gbq","phb","akm","iab","cdzt","cvb","tw","tywm","kzh","nb","gyc","ahp","mbq","bxcb","qkcv","bym","ig","xhcb","cm","bkwc","py","hqwb","xnb","kzcp","kyci","chb","bpf","ybj","hzgy","gd","wtqn","ccb","zfdx","tbhx","vwb","hmb","chw","wx","gycc","bfhv","pb","zcb","wkbc","gcw","mnyp","bm","xtn","bngh","gzia","bfhp","zn","jyzn","tiy","ah","zb","ib","hbm","znk","fqa","hjca","gqa","nvx","mpx","btx","hzhj","xhw","chh","nbh","hdg","wg","wn","gq","hbp","hjxb","zc","hfv","qza","vgh","bk","ktdz","bmig","cpwa","cah","wgyf","vxfh","yi","wz","itad","xwfh","dzki","twq","qig","pvx","hbww","kfny","nwxv","dq","zay","yadw","vcqb","gt","jt","cwn","fch","cby","hyv","hmi","jy","ycy","kfyi","cyv","gw","gpc","bj","bwh","ptx","knhx","fbhn","nj","ipnt","yw","dtcn","bp","yv","yybf","cbnp","gjq","nqcb","pqv","fc","kwtp","qy","dab","yh","qjwy","ht","ygaz","pvcm","yn","abp","icxn","cpw","qm","mxnh","npyh","pc","tq","bq","dkiq","yi","pxhh","vwz","txfq","ndhy","zgt","wi","bzx","kn","wmnb","xdhj","qibj","hfb","af","yx","wh","znb","qj","wcg","ia","ywkb","gyqv","cqay","wbhc","npv","ka","fct","agvj","in","wgbb","dw","cw","iyd","bj","gzpc","cdyi","bm","nf","fkbj","km","djfy","fy","mw","jnq","mxhi","qvcb","tc","fq","wify","xit","fcby","ywx","jhc","ybwy","bkq","pfcm","zg","bkv","fq","fbqh","bb","mhcf","gqi","twb","dvta","vy","mbb","bwc","zcga","xchq","jw","xzyd","xwfy","fwj","vzdb","bcpc","bdyz","yt","tcp","qzbc","cip","fa","hypw","txwg","baqc","qxv","baz","by","hizt","ca","dh","yb","hwv","ig","yt","zdn","xmzj","gdzn","gv","yi","ph","qhw","wbbm","yhfc","dh","xf","pq","wt","jncx","jwz","jv","ag","hnwj","cfic","qdi","hzvt","jagx","bqb","mwxb","bz","bm","cyhc","mv","hpbz","iat","mjw","chyz","zd","zxw","xvqt","ykhv","zw","xwh","cntb","hvwy","hh","pf","cwfz","ykd","azqv","nhgc","tx","wyw","pxhd","itwd","gwkv","qc","pvww","yp","wm","hh","bwif","bqnh","wtya","pig","ghc","db","jh","dvz","hwi","bgnb","yc","hc","tkp","tzg","badq","pb","miw","cmgv","nap","bcah","mb","pcba","fhwy","hwax","mdw","bhb","abcw","qh","gyac","iy","nzw","bbdy","icc","qkcy","ypvb","wpxi","ydj","yw","kbfb","bywn","txq","qhg","hxq","nhb","ct","bz","tdwz","fjgk","yzm","qbn","iykn","nzw","cmzf","df","ycx","qk","hy","qmah","bjyi","tdw","cb","jh","hpb","adfn","bjbk","dva","fgvh","vygt","inw","gq","bwb","wdq","nxtp","dz","gw","ad","dtyi","zyn","ag","bgcj","fx","ycj","jfzg","pha","thm","ja","jcd","ht","qcww","ibny","qvc","by","ypm","wvh","fw","bacx","cnv","yn","yy","wjb","bnga","cdpi","xzj","fn","qngb","nmjy","bt","dam","wba","cx","ngbk","byp","mpjt","yhvk","bc","czbx","gm","ndap","kb","bh","wbcb","npbb","txay","hym","gft","vik","fbai","ihq","wav","bwg","jpm","zv","gjxw","xyi","bjqw","pg","pyx","bthm","bd","ibj","pihy","hi","tnkz","fhz","yb","zgn","yg","wjy","iv","chwf","bzb","whfa","xbhd","df","cf","bb","hndz","ngj","dzcc","gtb","fydn","kc","dt","ty","wt","hc","ikbh","mkyg","ina","jt","dajk","mi","ni","qhz","kcby","bd","yi","hb","ixhm","td","qhn","ac","dcyp","hvjd","gy","jqx","hfj","fchq","bj","ja","bfn","nbp","dxy","dviw","wj","ajm","cx","hhgc","hpy","bct","bq","hxb","hby","bhq","zahh","mw","dh","ktd","gh","jh","jcpv","ckhm","cv","bcwj","wg","gkf","wazk","cbf","mybb","ybqz","bj","wpkg","bi","ygq","cc","nwb","jhn","qwhi","pv","nk","gt","kfhd","hpkb","hyca","ybd","qc","wb","xpc","wi","awdb","gy","thn","bz","hz","ca","vbf","jtic","wqzw","aqf","ghtc","bfp","ykv","gwaq","xp","wk","yvq","qf","pcz","vf","hcb","bf","cat","zt","phci","bwfc","ytc","itwc","kqd","wbw","mq","hb","bdk","yhgj","btx","kt","wbm","mvg","vwh","yb","pdyt","bywf","vyf","bd","dcy","xndi","by","pdk","cgwh","vghq","hjn","gwz","vhcx","wb","mq","bbpc","wv","gt","pb","dp","czbg","py","kygn","bmha","hpfc","bbjh","bh","hmzb","pvg","fxgc","nch","ck","ih","tw","wmaq","bxf","wb","hca","vg","hyg","gh","tcf","fw","na","whp","vybd","bwt","bc","agvb","hcvb","dbv","ywfa","ibch","cayq","fhzp","hnwv","ykc","cwny","cpm","bwyg","mfzb","vwf","fb","ma","wnx","jpx","bwf","fymt","vwxg","wdc","yqfy","xihg","tzwg","dp","fhv","zy","hz","yfx","pdm","bacz","pvg","vc","wkb","dvk","htp","pw","qhv","cxmg","bvc","bzay","vmzw","aibz","nt","bmcd","bvhw","wm","gbbh","biad","bna","yaxw","wyw","fpvm","ww","gcbh","tn","pyhj","fh","pbj","hmw","kw","pf","zbn","bhx","cby","gb","pxjc","nhy","pkb","kbwy","bc","at","jb","ya","yh","hanc","xyn","ah","qw","ygid","ht","pbb","wqb","tbzg","myy","gib","gy","cyh","gib","xwmk","ji","qwt","kbfb","bb","wkah","aibm","ab","hi","kajz","wd","gip","bj","ckbh","niw","zi","wc","tcmk","anib","igt","ipdh","cmby","gqz","fxqt","bhhy","gm","bz","md","fxw","tdc","hfav","qbna","hy","dxbc","hwpb","gz","ghm","hvyw","ip","ftba","wq","py","dia","ik","dj","xv","pn","bgni","fcw","bwih","ybi","wnc","pb","xy","bkyc","bia","mxcg","bfbt","dw","jc","ihcy","fvi","kjt","hy","pi","imp","mkiq","cyaz","jn","pky","ayi","abgx","ct","dn","fw","ygyn","byf","tbzy","igcp","ka","ydbi","ytib","dc","fp","pjf","nb","bwwb","ytqp","jh","zk","bj","fp","byig","ywqb","wy","pcyi","yc","vzb","gq","cpth","jygi","wyxh","kivy","bjw","nvt","yayb","hpz","hgwp","hxc","xm","wqdn","fb","kb","itjn","pbkc","gmt","btd","kvz","hz","vpb","gvj","fwtc","pk","hkcq","vcb","wbb","hb","vc","yifh","bpk","vfjx","yf","dhf","vfyz","xhpb","bzb","cbyi","ak","bzcd","hkhz","ph","ha","fvbh","bvb","ifbz","yd","pmcb","yc","yw","hptd","wb","hg","wxb","bhw","yn","bq","icb","nhhy","ync","jymw","bgw","xg","yz","wz","bwcm","vwi","mab","bdbn","njd","mbb","kwa","iq","vg","hi","ztjb","dg","wbhf","dw","gfbb","mb","my","im","kcvh","cxv","fby","xbnc","zvp","yg","pay","vbdi","zq","bxim","hck","dg","wbz","kg","mp","ww","kq","bb","ci","ib","xh","haqn","dnh","tb","bhb","zia","by","yb","mt","apiw","fc","ktv","ygm","gqb","ij","bpca","nt","jhgm","zbyx","mytb","ybi","bz","bvxq","ntzj","cby","abp","mvjc","bwqg","ix","gbd","qkb","icm","kb","mt","qc","cqy","wqzh","xn","gt","an","xh","yjfb","hcy","ccz","hxwi","ypi","mz","yb","zcm","dwbp","gba","ycbn","fiz","qck","yj","yic","ihk","im","qybc","tza","hgb","cw","cmz","ycj","gzj","xyf","hfp","abcz","ifmx","nh","zc","gfw","kmwj","tyn","cbnc","cjt","fwci","hywn","ghv","wn","jvb","mb","pdbm","dt","ihbw","wcb","txp","yn","xc","jy","vxt","iwa","fyv","qv","kiqy","bmg","vd","bnbj","wvh","nvxw","dbw","zp","cgw","bxy","bb","jhb","pgcy","yt","wcwg","ywbk","yzq","jh","qgfb","qyb","byfh","ah","qjyi","gxny","hxd","pwb","kg","cwkd","xw","wz","yhbh","gb","cf","xwqt","wc","dw","gqhh","npb","db","tzhv","cqt","vyd","yn","bi","zbpq","iabc","dbbv","bzwp","gpk","tc","wa","bzm","at","hnib","kiny","hxz","bj","mp","bqay","zim","hyw","wwym","fjyg","ax","cbv","fc","qbc","dwgm","xcmz","fb","yxfv","kb","yxz","whzt","bcy","dyk","wc","wap","nw","btc","tzb","qc","vnfz","zamw","kmyz","czb","piwh","xyp","xti","hbm","ah","yc","nq","fym","za","bihp","adc","db","xvjc","dkqb","ibf","hw","bmbd","acq","yb","yfzk","wgf","avw","yc","cxz","vt","ykb","qxh","jawc","wdzj","bdh","bqp","khmb","bwwc","hnj","mfkz","yw","jw","nyaz","bh","bba","icjh","xdp","tn","hfw","bhmf","kh","dbbm","xgbb","kac","zf","gcqk","kf","by","xhhw","xmiw","whw","abp","xvj","aczy","nm","dcm","mb","vb","gxit","gw","ibh","fh","wvqh","nyfw","vmpi","hkgy","wt","nw","zwaf","ahwb","bqyp","jghm","hxzm","hwdc","gjyn","tqyp","bj","zyik","cm","jfp","bhxh","pavc","mcb","zw","yjmk","bfx","gfbx","xwza","twkj","awd","bahd","xbqh","fwa","gzw","wbam","bt","bxdk","bhn","xwbn","bz","njpb","wgc","dpf","fz","hy","pwy","wwth","cq","jibh","wab","xb","zy","dnxc","cab","ycc","ythc","hwcw","vyfc","dvcm","naxq","fh","amwh","ih","tq","wn","diby","zyxc","fq","tc","bpbd","pkqg","jwyx","iyv","dq","gm","hg","vi","tcmh","mhfw","hpfx","qzjy","xbbv","cbaw","nf","hbz","nh","vgh","hyj","bxw","natk","cxwb","iy","mwb","qkxb","wfd","zjyc","fmyq","xavy","wy","qv","zh","hpnz","zd","ybnc","kwv","mhb","nv","bhx","cm","vkfj","hyik","by","pic","wxmd","ah","kdw","qhzc","vyi","wgf","cy","cg","cqbf","hxdf","wgwj","hwnt","cc","gbbq","ay","cd","gb","qz","gzm","hzc","qdhg","viby","pt","pvwi","iyb","yg","afy","hi","ybw","pb","jy","ch","biza","fivx","qdyk","zjyb","vp","whpt","yyn","cwat","jh","yb","gcz","mip","dh","vn","hnbv","xzm","pb","yc","nkxh","pcd","hfc","qc","fp","va","wk","wjdy","jq","gi","mcj","mt","wbyv","bbb","vzmb","bx","hyxc","xjv","qn","nb","cvwn","zqth","vbwj","dkqc","bbc","vj","yia","hd","ypxj","yt","wihy","gbhc","qc","dvq","wbc","kwbw","kiy","hhw","wkhb","jtc","chwi","tdj","bdtb","hany","bcqh","dpf","kyd","bzj","hhc","cdbg","czbk","gvc","yzcn","qgyh","wbg","hby","myyg","xf","wabm","yxja","bkj","tykn","xn","cb","xc","in","wc","pwwq","fap","mgb","jbb","nz","fbgq","nch","bz","qmcw","wh","ca","idcy","azn","tky","dj","gipz","mbvn","bc","cyh","wcng","iww","tc","bbw","dqv","zcb","fd","yxa","xm","tbv","zq","wcx","ijcw","bptw","wb","ib","admh","hb","ibw","cpcw","hbk","txzq","bnf","wd","hb","twg","pbd","ciby","nwc","ckdx","dh","yyb","bp","qt","ax","di","bjf","qcwf","dvw","ax","wtfx","jy","if","bp","caj","yf","qnhy","cw","fmk","ixj","hytk","vh","dzym","vy","xcta","wfw","yn","qy","jv","kg","xi","iz","zcdy","taf","znxd","bxc","widk","vpca","aqzw","bta","hp","ph","ccp","bcng","xby","wzk","bf","yb","ztdh","pf","wb","bx","vqc","zb","wv","xdbi","pzx","acv","kccy","kx","xtb","wgxh","qac","ka","df","zwt","zfv","xbc","zk","gw","yngq","dhw","byf","hyv","bnd","nyb","iw","zkt","jfb","wgyv","xhm","ybh","hhn","wx","vz","miw","kyxc","ikby","bqbn","bj","im","cat","am","igm","wqij","gv","bj","yw","axcp","hfy","tgq","kw","cgkf","tn","xmh","kvh","nj","kb","jbqw","fbiw","ca","pqit","tj","nq","nwq","twg","vbny","vy","wc","kiv","wihn","ydh","yyw","ca","yw","yyhc","cghy","qzw","khm","kzd","kvd","wk","hfmx","cab","yw","xvc","cwh","kcz","qp","fpj","taci","bnpk","kz","mc","wf","vfjb","ck","nkgv","bvz","wq","fv","yfyw","pqy","zfva","bx","hbk","bgv","jc","wb","bwpc","nbfd","hcn","gh","akbi","qwvp","khp","bb","bp","czjf","hd","dwj","vxg","iwg","fh","cyqm","btc","bjha","fh","wt","hfky","phc","qhb","th","ycga","qdm","xt","mvb","vg","yb","tdh","kbg","hv","awby","kdgj","fmw","hv","cwji","mgxk","hcmk","pcb","ki","ch","xfna","vch","gv","jiab","zhw","vph","nk","dmqy","jz","dcjb","hjyt","bfzt","vh","hpjd","vc","bk","jbz","tnb","cm","jap","hbyp","ah","wtn","tcni","btq","wyt","hbjc","bhz","ybnc","tybh","vcmy","hb","xyh","wkn","bnkq","tgc","zci","wca","bacj","gx","hj","ybc","bjmk","icxc","fxna","bd","az","fjq","wym","kbxm","cbm","bk","hwi","ywbk","iy","nhb","qa","jbd","iz","biv","xh","yx","th","xphv","mzhw","iwy","ipmg","wgyn","ha","wgxq","tfhn","fgab","ytkh","zdci","xi","kzgw","hdm","ky","awp","aq","dfw","nf","wz","whcn","nywi","xiw","mf","ya","pjwa","mpdc","pya","tgvh","tqai","hjzc","xw","tyqa","fba","cyb","tcch","tbc","myg","pa","znjd","fdk","dic","bjh","ig","mw","idca","yd","icb","whb","hwtb","wq","dzp","bht","wzq","ah","gcmk","xf","mpb","ji","bib","gnbt","adp","xwbp","hxw","ybgb","wjhb","nwzf","tyh","zmja","bpi","qtzw","wg","tfc","bvg","zp","igbj","gh","hxfy","thby","abc","cknz","bqh","cyxb","pbhz","qih","xy","ynh","bfc","vfmb","qy","dwcj","dpbc","qkbb","cwvx","bf","vm","hjft","bxd","df","bck","xyb","and","qfdp","kmcz","bibt","wgp","txz","bg","hyfh","haf","myxc","gnpj","wmd","hj","wmyp","gt","civm","yib","mwy","ibpt","ch","azy","chjd","cbmt","baz","dqv","hjzn","wzcq","bt","hvk","ypvw","nhc","fb","yxpb","cng","nq","dya","vmbc","tcc","wbd","yafb","hpm","dv","bw","pm","fyz","kwc","iwfb","bbk","hta","wg","yj","ydyw","wi","kg","fyb","hw","tz","aigk","bcb","zh","fxkb","hzfh","xgv","bbx","yp","cxf","aw","nzfa","vcp","npf","ybyt","fmj","bhwj","pc","vnk","cxm","wzxt","cqb","jy","kfb","iyxb","xw","hiw","bx","zabp","bnh","ba","tybh","gytn","dpc","caf","fw","ghpy","pbhb","wphd","yb","wycn","gybf","ifv","jv","tbb","an","hz","zt","hy","jqv","qnw","fvk","yj","wgn","hndb","bc","chh","wipy","vhw","wb","btv","ytw","nhy","cxaw","iba","zhd","cb","wf","ib","ak","vqjy","nm","kz","agt","znmh","dcnb","dxw","bb","pq","cyph","cjy","bqg","jyvk","yvwc","ydp","hjpw","jxkc","cwxm","bxwp","vj","wv","bqyk","gfby","cy","tb","cv","ctbp","vhzm","bvc","mx","ipz","hb","cww","iza","zh","kqnd","cqw","bt","jc","bcyi","qjhw","hz","hhiw","aiy","bmk","mbyp","jcb","xyzb","bfd","wxgq","zwbc","bi","hxp","wyqz","mctk","dtw","whb","pbih","yxbn","gt","ww","hij","fhg","bng","qpx","apv","niba","bq","fqwv","ix","ykwg","kw","jtyf","nh","zmck","yb","zb","vw","fty","mi","dbt","kc","wyhy","ib","zm","dwx","mwjy","tbjw","bkg","yfcm","fdxm","vfdy","by","zkca","yyg","nwgt","agh","bwhd","yqz","zthb","bwjc","tp","fykn","vhad","qba","ybh","ba","kcyt","vmjh","mcw","bwxg","my","yb","jbbz","bak","ha","hk","bw","yq","gh","vwt","bh","zqyb","dpbv","gj","yzf","jm","dzg","pfbd","ycya","ba","jcfb","ciz","ty","vy","wgby","vqz","wba","gqn","wky","hj","yzj","hht","ibj","ziyn","yzj","jgy","kjgy","pnm","wbj","fdwh","hw","gbpy","zb","yxd","pt","dbb","ixf","bbzn","hgwy","qkww","xmwd","yzw","wbkx","wpgh","cyqh","ch","htk","yw","whyg","pbd","kq","cc","zd","ypiv","xhp","iwz","wcjx","bxm","hh","cn","yy","fy","qhd","xpq","fk","hnaw","jbik","cy","zwb","anwz","ycn","jf","nvty","qcn","wmc","hybq","ywx","nbch","hmb","zybb","ayc","hqd","pby","xyz","zifq","bqw","bp","bhch","bfh","ftz","px","yabh","wvc","kyyc","nmbb","bcy","bvwa","gdxm","ydyc","kdy","wbyf","jqyt","viwy","xqcw","yybp","yhy","cjx","ytwy","bjb","yznb","wq","yw","cnyx","fab","xc","qhy","mpb","yfw","fk","xhna","nby","bgny","bbzh","ckh","bd","zvm","ywf","wzd","zcvp","cpxh","at","pdh","zh","amth","bhkj","zbi","hy","my","gcdb","gdi","nw","xmi","qbd","wwch","mnw","mj","kav","yyth","yngd","bpyf","vy","fid","hz","bh","cb","wydb","jtmh","bgxb","fbgh","chyc","ccdg","tvx","na","gn","xyb","vx","ndwf","ndkw","qb","bgh","bd","ja","kd","cq","wyfv","tyq","jb","dgn","qmgz","xac","cvh","cqnc","kh","qpac","izx","ki","hgb","bvb","vhb","wd","ati","yjw","phh","ccmb","ipbj","qby","jbb","gcbb","cbd","fwbm","dc","ajb","waj","fh","hb","zknb","hkim","dgb","bypx","dhx","zcb","jwf","hyc","hb","ybbv","pm","bp","bw","pcaq","yqah","bbqw","cb","bq","gi","whkg","ht","mb","bq","kf","bkj","db","cb","afmk","igw","ih","wgbw","hvc","axwh","acg","jbnb","ynwa","bah","wygb","phc","pvb","ybgc","ytg","fjc","icy","amyf","yb","wicw","mzbd","cnb","jbb","vc","na","da","hbtb","gpij","ybh","kytc","qa","mbya","qc","ci","hhif","dgbc","cg","ihzc","ymwd","twph","qb","ch","wzhy","tca","xcj","wjfc","wcn","ywz","jb","xqjy","jyzb","xjb","if","ikc","jvc","jbnk","pvfb","hm","hc","iwyj","ch","ndb","dwm","im","vd","baq","tcw","hkhn","vcp","gbq","iyc","pywc","mw","cba","yayb","khn","cjx","zfp","cmg","yi","vmhw","fm","bh","cz","db","kbh","xbdw","abyb","bakc","fgz","qwb","hwyn","xpy","vcjb","zqac","igq","mxb","kbxt","wyb","ww","btiz","cicq","fwm","gvcb","mh","awwh","vx","ytv","mci","yp","jbyi","dc","vb","pn","fh","wny","xt","taq","gw","ig","wn","nkj","bf","bmw","kaf","tnhd","fyht","zc","xyh","hab","zi","wng","wtvj","xy","ic","tifz","thgv","kfmj","xmwb","zhay","mgf","df","by","jhn","th","gk","nk","bxy","vc","bz","hk","ynq","hm","hcc","dbtf","vg","bci","icwk","qtj","kj","xhtg","hc","ykfn","jbky","nwyb","vw","pb","qyw","fy","ydfh","qv","cd","hgb","hww","az","maw","tbd","gcb","bdvi","kcn","znhy","ac","cidn","bthg","qbix","ym","wvq","an","vak","xyib","hcw","cb","bc","fxhz","gq","xbq","bhmw","iny","yh","jbx","mwz","yghd","fdxj","njp","xqzh","bnzv","mzib","qwjy","zb","bpcv","xvp","gyp","hcb","cqz","dbw","ywny","azfc","gzq","ima","dy","fxqt","vp","hcwb","yz","yap","wbhj","aiyc","wwb","yc","cdy","jnq","adg","naq","qyvc","gck","wpn","cadw","wyw","nh","nqa","jm","xyat","qtzc","yd","pwb","mdcz","ybx","ptwh","chtm","gh","xiyt","yzwb","ywz","pcm","wy","wfix","bdw","wfc","ihw","ntb","vbh","hwb","aw","ctb","yp","wdbi","gf","fybh","abdz","hy","zbib","qbzb","hbzy","bbh","gw","hwy","yy","vj","mwk","ngc","ajkx","in","cdjx","hf","bxy","yz","kg","wmg","fhbd","jd","wv","bxd","bvw","ihwc","hfh","wb","kn","acc","hazf","xy","bjz","jbx","yzn","bw","by","iamq","ycdc","tdp","njfy","fznp","wz","akvw","cafi","wym","thmv","hh","ny","tmv","bphb","abvb","bg","wfv","xc","qmh","kxt","ach","cp","iaq","kgy","hdwj","cf","nbtd","qnw","twcy","tnwj","kwb","hhk","bh","mjcw","btm","iyjt","mvz","iba","zjv","twxg","hj","ma","fwh","vg","wzh","wvc","ak","jc","cmik","kwm","fjq","hby","dnw","tbh","ya","qcwd","kv","mbaf","ywwb","wv","gwab","af","vx","za","hw","bjb","bqch","cgnb","bwcz","cwdb","ag","nw","kc","qj","cy","cdw","tid","fn","cm","cnbm","gi","vycc","qb","yk","cwy","tfh","bh","gf","yfa","pymk","yq","vxqi","bq","bi","qmaj","cpdy","ihb","wcx","pxy","tqaz","kbfm","kfcc","yyp","bk","jiwb","bihm","aqjc","cb","vg","nivb","ikv","gc","jcy","bi","vx","xhj","gtkx","ptdw","ihyt","fh","bqi","vcx","bjn","bzdm","mtav","cy","vtbb","ctvw","bnhc","gthk","vc","ykan","jz","xhny","wfh","znw","zh","cnyd","bn","fymn","fnyb","ijf","mvc","df","zwf","ytq","cfi","ybj","mw","cy","htq","hm","pm","jphc","vhd","hz","by","fxb","avnb","cv","hmy","xty","qv","tf","jkfz","wmji","zkq","hwv","gw","hacb","yych","kdn","whk","knp","wzi","bfy","fhgb","hbm","czw","phjy","cb","ybct","hc","bcn","jm","vfw","hbh","pta","tcpn","jyg","ytcm","hbyn","ct","cb","hvp","wbxa","xcd","tahy","ajwh","jb","ywk","ty","azpn","bd","mb","ibc","dvc","bzny","dbbw","iwc","wj","iycy","wh","bhba","tb","qp","pzbm","vyby","gcd","wp","fkh","yjpb","bydz","hb","nb","qft","nfkv","tiy","zh","bqwi","yb","pyf","hb","zi","vq","wxa","ahw","gn","gck","xk","bf","wmx","yz","jhxz","hd","nfh","pnya","zj","vyjb","hhn","hig","iycq","bcq","ci","xibh","fq","gk","yd","bmz","bi","mb","hcbh","hgdy","pch","iht","jvy","xth","bqjv","bb","ik","wy","gb","dkp","wh","wkcf","bi","ah","kht","pyk","hjvq","bgyk","jw","hjb","ipb","ipb","znmj","by","bbb","bk","bywh","jwd","pbh","nfkb","pb","id","fbb","dngy","hyh","xcb","aq","aw","bvk","wc","apg","bnv","hcqp","bc","gh","cc","cy","ti","bh","bhb","mgby","yk","ngwb","hh","zdx","jv","ty","vga","dxj","dwfk","gk","nh","qwhc","bcc","vfnb","nqbc","fi","qwv","by","mfg","wqhk","kaby","pn","ybz","ny","yp","na","gx","ayfz","fb","dwx","hdkc","ycji","ih","kcj","md","ackd","gyqd","zkvj","wz","jhk","bcn","zhw","vz","mwtd","zk","bh","itwv","jc","cqj","bwzv","bixy","yn","ahw","mg","ny","by","wy","bk","vzwf","hj","dyzc","qvht","cxzy","pbn","azh","bwkg","axyv","dbkv","jfp","vzq","qb","wz","bakw","zv","khph","thb","hp","vfk","iy","bcxd","zc","wa","bcd","dn","ay","vcnj","hky","zh","tbi","zty","nvf","bmcy","wyb","dmi","hktb","ahh","nk","nhy","jv","taxb","tzg","hatx","cbn","cwgn","tk","djf","wycm","ca","gixn","cqk","hmzc","vcfc","thay","bd","vyw","djv","mbty","hgqj","kqm","zh","jh","zc","ma","hbm","mdh","myk","mywi","bzhb","xd","cpk","nqvb","ymzc","fpgt","ca","jbyw","vz","zywc","gvbb","qyxh","fd","fap","ktpv","pf","mawh","yjgt","cy","hgib","nqzk","cb","hnbb","gqhb","javk","qdyc","md","ydcb","ihb","nic","hkw","hjb","wpx","cwng","ftm","wkcm","bwqz","jyt","fbh","iqp","ckh","mfzy","xq","fpjx","bi","zd","ht","pbwn","tncw","kag","dykz","wcc","cc","tkpc","ap","hd","cbth","cyat","gty","ajxw","jyc","hiw","mw","dmbg","kc","hwpz","gbf","qchy","xhac","iga","pq","hk","gzqb","hmz","hfxh","htzh","gc","nxpb","jfy","nyd","vwhi","wagb","kt","bzn","vpi","kcwx","ji","cypt","xcjw","dxfn","bdi","hm","byqw","vhb","ctpb","tc","nfi","qcn","nk","wqh","ih","hdha","ba","amq","bgba","anxi","za","wd","ijt","kz","qxbm","hbyx","xcv","wp","mhc","gyc","yw","ic","gp","djav","nhb","anhp","qwzw","chw","hyct","imbk","btn","ph","wwkb","hg","tpfk","zwyq","hcw","xg","ihb","wcf","hiw","gvk","kw","ihzj","dng","kx","qpb","bbpq","nq","vg","hw","pbn","phdt","xj","vbci","fvc","ywq","dbx","cwy","ymv","dbqw","cah","zn","htmz","qpmf","yhp","jg","hzbb","gzxy","kvc","nbk","qf","zc","qyp","bn","pya","at","pjt","mfdh","mt","chyw","cvxw","dz","ax","dqfb","cbqc","amwn","ymh","pfcb","nyq","tic","dkqt","hhb","ncbf","iwx","ybch","nh","ywbb","dgb","jcd","nd","xijn","bagc","jptb","bgk","twxy","ybcb","zqhy","wdb","wzmt","bck","fd","cjbx","bigh","xh","qb","bf","hjzb","cyp","fhw","kcfa","qij","ngak","mhdc","itbh","cbb","iwvb","bi","tpb","ik","pfq","zn","ycj","kz","jcc","ynw","hi","kbc","gkpv","jbt","igh","vjp","bgt","nyyz","wfn","bw","aigk","an","btnb","vp","dc","hny","yi","npdb","hwk","wg","cdv","pbh","fpk","yag","bixq","wj","wnc","cbmc","nx","fm","zh","nbx","gbiv","czh","ybwb","acx","fw","byjc","tzhw","fzmv","tmja","zahw","acy","hyvf","kpz","mjk","jq","ccq","gby","bc","kycw","tchn","fij","gvkw","hmyj","kdah","tb","hz","nbq","id","wcw","zgk","bpb","dbjf","zn","mcwh","bcph","hpc","it","py","pmic","ycn","tvcx","mj","bmj","xbw","cq","wt","bb","vtk","qz","mi","jn","iz","hhqb","cw","bk","bv","tcj","by","bqa","dyhm","bwn","hdbq","pwb","wb","mv","wfdb","ymq","hp","hnyh","wng","awv","kyd","zgq","yn","zyj","aby","jd","hbi","ty","xbnz","mc","bxp","wh","hvz","mihf","tq","jw","xg","yi","py","zqf","ct","wpb","fjqc","yv","wtyk","wjkh","vyx","wahv","bzj","tz","av","dh","qx","kx","nhqk","ihj","xb","id","bi","ygqd","cxm","jk","itcj","iwcz","kyw","bi","hvj","kc","twbj","bp","fjwz","gb","gzn","bcx","ck","zh","yhma","cyip","yvb","tgcz","kbvb","bw","qi","myc","xbcw","wp","ijb","yhv","dka","bwh","mby","kjcw","qydt","ag","wbq","dk","zhv","kb","wdg","bdax","wcb","jn","bt","mwj","fan","bnp","zmf","bmh","bw","qhb","ckam","pcmh","nzc","cm","vq","icn","acyq","qbg","zj","yaz","fnyd","piyh","gi","yxz","qzab","phn","btbd","dbap","pwmc","vtb","bqi","xmbf","fyzn","ay","bcf","ic","cyn","bvj","cya","hc","wmwh","mfwz","mwn","yq","xn","bt","bqd","mjq","bjih","hi","wxy","xig","icdf","mpy","bym","cwwy","jyph","ja","yz","vyw","vbwp","ibjc","mvy","qjb","ph","pmb","xw","dyy","wti","by","yn","nk","cz","wp","ymtc","bvkj","xvwq","nqy","cctx","gicb","mzbx","bgwc","zx","mbb","pj","tvwc","pa","zfja","mbv","hbmh","gb","jvmc","qd","bih","bdp","cf","gjz","cpt","wfjh","yz","wpb","fmyc","yvwb","pc","ahcq","bx","ypfb","dcxm","yw","zbh","yb","ayfy","pwc","vqm","bq","xag","ac","jv","fb","wg","tp","hjiy","vbyk","qpg","bnby","pj","pjkb","at","acb","bct","cjb","wp","xaf","fwt","xt","bqxv","nz","vjbf","cnxp","dz","fb","fg","hwwn","ztp","mgqc","zjw","gcfp","imyj","yt","vyb","axb","gz","jd","yyc","yz","hx","hy","kjx","wnyx","by","havt","hzdv","ccg","dmy","fi","cb","gjax","kb","yw","fh","jhwb","kchi","cdj","nhqb","gbfb","ydfn","byk","nbiy","hapb","vp","zhdi","fky","mh","gqy","gm","ctm","gbj","bxw","hcy","zip","mv","jdhc","zfb","wyc","cywx","twwi","mkf","yd","yb","dmb","hiba","pmhb","fkz","yh","cpz","tip","jy","dm","xbhf","hqzk","fpi","ctz","jhgn","nj","nbwa","ktay","qj","qa","jnyz","py","zybh","xc","qb","pd","mnkh","fjk","wv","hh","fb","cbxp","ay","cbk","zxbt","icax","ink","mxy","ajyw","yq","bbmh","wqp","wgcq","zbpy","ijnz","xc","px","qbah","zmjn","jc","ybp","ct","inhb","dw","mi","nb","vj","bqy","ci","dnbq","ckqb","cqwy","hx","bwqc","iw","qa","bjcp","nyw","tjh","bhzc","fy","xgbw","baj","cyag","jnq","axwh","bxq","mw","jx","hxg","whi","hvjm","qpty","yh","jtv","wtcd","kxw","xmj","fpcd","dmip","zhy","yc","gjb","bbnw","ifm","bdcn","ytp","hz","qp","hw","ciqw","dhp","twb","dwhm","mb","pzkq","yc","xhw","bi","gj","nzb","kc","wq","bh","mq","fqct","yk","jtv","tghb","wyb","nj","pb","bmc","qfwt","ya","xb","xwkg","wzm","yywg","wgx","xcyi","hywb","zafh","bd","ct","htwd","ghvi","bbh","vgc","wwx","cawg","whah","yv","fnkj","vhd","vwhd","cmvw","vhf","bfwn","xh","xbqc","vzbb","qc","jbyz","nw","wyp","bdh","cbhm","wc","yd","tqb","hd","ny","hab","pbv","vhx","kwd","zp","bi","czb","mq","fc","fh","qhjt","wbcd","jbb","vawz","bc","bf","dxy","nya","yhj","qdy","ztp","kn","qw","bbc","ct","pi","hk","gf","kc","wd","bf","tbpz","yan","bw","dt","wx","gbd","ntvd","fgjp","fw","pnt","wgbc","gf","ibkb","cyji","hy","bhw","ihk","hbwv","vabk","zvbh","qid","qf","nwav","hd","hfwp","hcbb","cdh","bqg","btwa","qgy","dniy","chpn","cymz","gd","ih","bphi","wzax","jbdc","qy","gwy","ybm","wb","vha","njha","chx","hg","ytw","wbj","ww","ftm","bphy","mdhn","ift","ycv","dwx","hyvj","mb","bw","wtj","bzyc","yz","bkdh","gtj","nzbk","xc","gqhf","bj","acxh","kiwj","qkg","dt","ybcw","win","wcgx","qxhc","cm","mpc","dkjw","vbya","gp","chh","knf","tbhd","izbh","mav","bhd","dt","wbfj","hd","njzt","hct","ccmi","cwib","cxf","atnj","yvx","bz","aj","yfc","kcb","yy","jmd","cv","btbi","ib","xk","djyv","kjz","ixw","gbj","yw","yqv","ycg","bmcb","pbxh","kgbw","bjbc","gbc","tji","bik","md","th","za","zyc","nfq","tgdb","jfpm","wzpq","pwmc","hyq","gj","tm","wwi","nvkg","gbw","pcb","yby","kbqw","pbxf","qd","ky","yc","wx","cgtf","fyw","ydv","hidb","cvc","py","cg","bqn","hyt","vywc","xd","nbb","pzcd","cbmi","py","xinw","bbnh","hqvp","izm","wc","awt","hc","zkc","wydw","gcf","dphx","ibnj","gdzm","bqzw","wx","wk","jn","vhb","ki","tkbc","yqvh","pc","jyxh","dhy","cihx","bghc","xbq","iy","bdf","yh","by","bbkd","itqb","zqdc","nbt","xygf","cyqh","dpzm","wgn","ihyw","tn","nbp","wd","nyx","ty","cqnb","wa","vi","zkb","nxfi","hw","mt","bbyc","vhb","tczw","zm","cctz","ib","nc","iqcv","fyv","wx","ynt","fyb","pbcf","fbch","mzh","xf","kb","phcc","gcm","dh","tbf","nywh","by","gybb","kjta","wkx","dnq","ct","anfc","mbjf","ch","jbf","haj","txm","xkmj","fg","qmzy","dg","kzha","tcbk","fc","hbgq","dp","jv","qn","hd","xqab","dc","cthy","wjyw","jp","dw","hn","bwkf","wc","bwxv","zx","iy","bj","cz","dba","zv","jnt","bc","nch","dthy","wwn","ziv","fy","xk","aw","cybm","tb","pha","nv","bhtp","zty","bqt","xy","wkj","pykf","cwzf","fyad","ptw","vwa","pqyd","mxyz","whq","ihxn","dygp","chdi","bvwk","dw","tywk","zmw","ic","xn","ha","izam","yay","cn","cfq","zcdh","gi","ktcg","pb","fchp","bwq","hav","vhk","wnq","fhw","bn","hcdv","qx","mfhh","kah","ybwa","wbt","tpm","hm","ca","xkp","ypiy","fwk","fbw","zc","jx","nyg","gyx","xza","btcn","jzm","hcj","yimg","yj","wht","jqvw","tdf","vc","tih","nv","wm","wib","hmwk","hm","kz","bbg","tb","phw","mbb","bv","zhwa","caph","pg","bv","ixcq","akp","abbq","qwc","wgv","jct","wi","vtb","dfzx","bqcv","ycvz","png","tvw","kx","ycba","hpyz","vyh","ab","ywg","nyq","pj","ba","bzh","bxgm","xknb","gbb","pc","ygc","hzjy","bwvk","bnwh","ydcb","cwtb","ajh","gqk","xbn","qwc","ygv","whnw","zb","ybb","jdy","hd","aw","gh","ik","wb","tbbw","zjtf","chb","qw","jtpx","ja","qg","jwhx","ptkq","xzjb","kdmb","zfpx","twh","cbg","dk","yn","pjy","bt","bfd","jfb","xibg","apft","pvb","nw","jiyh","xthk","wbmb","bt","zv","bxqw","pc","vjhz","jdfa","czbf","kypx","ybcx","mc","gwyb","bciy","jdzh","va","by","kzgh","kp","wc","hjax","wiy","cy","xd","bcyz","yc","izwh","whfy","vy","wzcm","ytwk","bc","ynbi","mcy","bq","gdkm","dcig","tmhc","dv","wtqx","db","mbiz","itc","tx","jc","hh","wb","vpb","wtyb","hytj","hvmy","bzb","qbkg","zk","ib","ihd","jgth","zc","nh","cjha","byhz","wtbw","dpgf","qg","nxwc","mqj","ftcw","aphz","hc","jqbv","kmdc","zy","cvhc","cnbm","mxyc","hdyw","cpb","zfw","bid","bcpw","mn","zxb","wmg","gdiv","ihn","nw","cbhm","by","bh","byaw","mv","hgp","qyb","kjf","djb","cy","di","vbz","kmc","nq","xai","pwx","hib","zqva","tb","hviz","cb","cxtd","kbhb","czbp","cn","ckc","db","hta","pcyy","bdn","bycx","tbdy","whj","fqch","cq","qch","nbcj","vp","hhd","fj","vnp","by","wyjg","bwbc","dp","xmyj","pzty","yz","yk","dnp","jy","ind","bzc","xzwj","qj","pjq","niw","jb","ybd","tb","by","hj","mxcc","cp","dnac","zmx","cp","fw","ym","cbq","mwk","ybqc","bgcf","qia","vky","qbn","whkn","hpym","pbcx","tph","gbyw","yj","igd","xvbc","gbyx","nbb","ax","gfh","hw","cfb","bah","adbc","biv","cvd","fkdn","difj","kgyz","jzcx","jh","zy","qygt","ww","xcng","bp","hcxv","jd","kbtw","zm","km","zc","aywy","taj","chj","byty","gfbk","czd","iqy","iq","jc","px","hzb","hkin","vxap","pb","xby","hdbq","wkj","bwgb","pbk","mh","bf","ynim","cgp","ta","zw","hb","gqbi","xa","ahqf","hg","vx","cv","xgvt","wb","wbfc","kj","mww","pgj","yxi","if","fybv","bp","tcpw","ygp","bt","ch","wafq","cf","wbh","fgy","bz","gqp","fp","ywxg","bqj","hzw","gbh","bq","bk","bh","avjw","wvn","qm","mn","bzch","kyvz","jbdg","bt","bbt","tp","qgcd","yf","nb","bvxw","nijc","fib","by","cf","ybc","hmby","bbkn","pwj","wj","payc","wyxb","nciv","ndy","batc","tba","bh","cxw","zmfn","baw","mwp","ivw","ihzg","ht","dc","mxa","yjw","ny","xbpg","pfbg","hqck","cwjh","mpw","cwv","kzym","kbc","dfc","ti","ibb","xwpz","hhj","pn","jwaq","aix","iqy","czh","vpqg","hk","pbyi","hm","iyz","bd","fj","ykcf","xhv","cmva","czbq","fqct","icf","icvx","qkd","nzw","myiv","fynz","jcyh","kivd","kixg","cap","mcbq","bh","mhv","dga","nj","jwcy","ykwq","qhkc","bbby","mkb","fzn","hqmf","nhh","whdm","yz","ycap","by","zyxg","hpjn","gq","dzw","qp","cwax","bi","zaiw","ycgz","tfz","gf","wkdf","px","yj","cyh","hzjc","bnwc","mc","gfh","kx","ktvw","ytq","ydpc","kcbh","cahg","jihq","knt","vya","hww","pbyc","qwgh","yh","dwb","mwx","jqt","xbji","mbh","wc","mn","wwp","whvd","qti","pbbv","vnbx","ahd","wwgk","hxnv","kyb","pjfx","hf","jdb","cqc","qnk","cbd","qbb","hij","tm","dgcv","ft","yi","xicb","ybw","wyic","hkz","kyb","tyw","mqwv","bw","qd","wbat","nfhb","jb","wxfb","bkwi","ab","zn","nhj","yhhb","cvb","ymqz","dywb","cxqb","ib","whqv","by","pyh","fh","wkfw","bti","bb","yj","bc","vkyh","pvh","dmh","vbky","pq","vckd","pc","jpym","hz","wmy","xg","ycwb","ywb","knc","cj","qcbi","mh","qj","mdw","iy","jgc","ycjb","kfc","qxpj","fzb","wfb","nmh","vk","ajmy","xcng","xg","ifcc","tnq","wxwc","cm","fqyc","yb","yv","pcgw","ivbj","bbnc","qt","wzb","hbg","qh","iy","wz","wcg","awtw","hcdw","yx","wyv","qt","cjp","pix","vjy","qz","vch","gy","vkz","dh","jzh","gp","mxw","wkc","tbzy","bwmp","bccw","zhwn","bm","wdjz","avbn","ibj","htxa","mdy","kghz","pbn","thy","jv","pv","vbc","khm","xg","kf","bgqw","gh","twj","gv","zwyg","fmg","pc","dc","pfc","dfb","nd","hvb","wzc","qh","wh","my","wc","ig","fk","hbjb","dckh","ambz","qb","ib","ynaw","gbyc","ktz","vwh","bbgp","tw","xqb","qwb","ik","fb","bh","btf","kj","yd","bi","vb","bd","cb","nqmd","fjm","mybt","yif","hgdy","bb","ch","izma","vxh","wphc","cbyt","qd","fd","bxbh","bkbv","cm","tyh","hf","jk","tq","gbmw","qkc","yp","vnxw","xiz","bkzi","tpcy","ajy","yf","jd","mt","mk","cnx","zbwn","cgk","pnwb","bp","wfjm","hyxb","btx","qh","fab","jcm","dqpm","pdf","dth","jdzm","qtfp","iy","tp","ih","hfh","bwca","cbb","zvbh","wtcn","kc","fyw","zt","tyxv","bwkq","tc","mh","vng","hp","bkgm","tb","mw","wy","bcdv","pf","cfvq","qand","igb","hwp","tzc","cfy","ch","vach","dv","mx","tn","hbvx","hmx","inz","iadb","nwb","ydh","bgc","hkc","fkw","bwy","dbjc","bww","tbpa","ika","twk","giy","pcw","tdym","tm","vch","dxyb","ta","zb","pb","tj","cb","kcw","tiqk","kcd","tb","wbq","dyga","ybj","yznb","nf","xkwh","zt","mby","vpm","wdwc","kb","bdm","cd","chgv","tjb","hnw","ac","hcz","hb","qhtw","dwgc","bny","dw","yq","bx","xi","xbc","aq","aw","hnic","tgqn","xgh","yij","ybkw","ba","iqcj","ghyf","qbgy","mw","twjk","vy","ghb","hbc","pk","kb","bcq","czi","tc","dych","ikvh","fb","twbb","bktf","tdnb","kbd","bahv","mct","zx","ghtn","bq","dwbf","tm","xyb","jkcb","mfgq","aif","ny","agzv","mxj","zvy","cyia","dqh","qbc","hw","jfb","bqh","ybm","dkm","xbi","qpxv","ch","mfd","nfy","dc","at","xwmw","bc","wm","wm","ihz","vq","wb","byit","pc","mi","xwk","fc","njbc","hdw","ki","ctbj","hqb","ypht","ifh","hdzt","hdbb","hc","gwhc","mhnj","bh","fm","wbvi","wh","wa","yfb","gh","yd","iyvm","ntzx","pfx","cifb","zwac","bb","py","nykb","cdb","kbah","ckn","qp","gca","hx","pcxt","gmy","awjf","zchh","hbg","wbcx","dnpx","zcb","yj","ghbc","hdxp","vhh","zyc","fcwt","yybt","hxb","dz","tjgv","mt","yw","yc","dpc","pz","ytf","kb","cpfc","mdx","bb","ib","xkfn","ja","htc","mgk","hbhv","mxyb","yb","fcb","iw","mf","ndh","ihbf","jkzt","dxwp","ztn","vh","ma","picy","pnb","wnp","wxaz","fbkh","vkhi","cqk","bhgw","dchb","mhcg","vw","bym","hfh","bc","wm","vi","wq","mbdh","mb","whh","caif","hi","hc","jahb","ig","abx","ab","ca","ngvy","bd","md","tgij","cnky","aktb","tbhp","why","ajzc","kg","hw","cp","bqv","vt","bwza","bzh","hq","ty","fbi","mhp","xgq","jxww","iw","gw","nh","aw","nyx","da","cbd","acdb","jywk","zwb","zb","vakh","yqf","xay","vc","bhx","fqn","dyt","tfwj","pv","cb","xcjc","wg","pa","dn","mk","txy","cbyg","bbww","izqn","cyb","abnw","bvmf","xd","tpv","pcmb","jnh","dnjw","hcj","hj","npk","hwzc","mdn","mwc","hb","znhw","aw","hca","pcq","ighn","bcb","cwh","ab","kyy","bmt","cwf","wmt","ht","cgwi","cw","zhbj","ta","xhkz","wm","dh","pz","mbnp","qzn","qt","wd","nzkd","itkn","hym","ncza","wx","xib","fnb","gbbi","cb","qpb","hqy","fgx","xfn","ct","jcxb","mabw","yz","bph","dc","hbk","bpy","wm","iyy","nzfw","abmh","mwj","ywy","gw","amnv","tbh","ztyb","hcb","bmfh","byzc","fb","hayg","ypnb","xm","ihyc","yqf","ad","cw","jqah","kd","xh","xqb","jbvh","ghbz","mbi","it","abh","jzim","hgbw","cg","bbjp","mcwi","nykf","dvf","ai","wt","cfja","pq","yxya","wdjb","bb","dtvb","wpm","iab","av","tyf","ayf","ijh","hhpk","cyb","wnh","bz","vy","wphi","xt","mvd","nb","xbzt","mdyf","pbw","iyc","pk","dwq","wbaz","bd","cbdx","vqxg","vkb","tmbz","zc","cz","ykxj","ma","gx","vzjy","vhy","zhw","zcbw","bqwv","bkqm","czn","yqtg","xzq","wmbg","qdya","bwh","kbyg","cit","fnc","wv","dkn","bmkw","fpnx","fqbw","znga","fj","nzgy","zq","gnkb","pwh","wkgd","hfh","hx","zmvk","tqbc","hqg","cawh","wgz","bd","gb","kbgq","nhcq","qndk","mykh","cbn","bmf","bwh","gb","kdj","pbbc","cb","vqw","ybkc","zjg","yf","nv","yz","bwyg","acyw","wh","yyv","cpb","yw","bvpn","mkbj","xaw","zcyh","ygdw","kjxf","xfky","byk","mht","xa","yy","iywa","zibh","wizt","fw","my","zfk","fcc","nhvh","ndc","ygmq","bbp","af","jwf","hvb","gwyj","mvdn","wcyx","ckmp","yakn","mdnk","cpm","hpy","yb","vxk","cy","xcb","mby","vw","fyp","ck","by","fk","xbwg","cjnm","tbx","ink","zm","tvh","gtk","zip","pihb","qkp","bij","qkd","hcw","znha","cf","nwqh","icm","bynh","cabh","ydm","cbc","xc","whij","yn","zdht","fjgw","zgi","ib","xc","iyg","vc","hj","cb","yb","bxwy","hf","hb","fyz","gbh","gqnz","tv","ng","qfdb","jpd","yhvh","qb","hmj","iyz","cpbd","hk","bid","ywv","fjwb","ctbv","cizb","wx","jpww","bj","dp","bwyb","vb","xngv","nfph","wi","ibbb","czkq","iqzt","ty","ty","za","bq","bqi","wcd","wbw","gzx","zy","ixw","atyb","bhk","yqp","ybkp","ity","jb","fw","bc","fciw","mw","cdp","khw","ag","wcn","jb","jyx","kah","wb","mazc","bmj","wb","bp","hfyz","tyiw","xhw","phh","hpwh","hict","bjpi","bbwg","nya","jqp","cw","gwb","bxyv","jdt","txa","bn","apq","mt","avm","bkd","pyc","bc","hp","fmaw","gpd","bwg","bj","hytc","mb","hgp","cybz","nw","fc","wx","thk","xhdk","qjv","jyb","hc","gbnb","ayb","bjg","nbz","kif","cn","cmyd","ftk","ygv","cxgh","jncp","fcq","nca","bth","bmj","wj","wvyy","byi","kzbc","nqt","ctk","wh","af","bmbw","qm","hbpf","hvqj","zq","fbn","wvba","bwtv","zt","ni","chzd","awby","ifbh","bhv","qw","tkh","mh","hmb","hv","cnbk","dfba","pdwc","gacf","hy","ct","pcwg","yqk","gamt","cp","idyy","gvy","zbdq","bbc","dbi","bjc","bh","bvz","caq","mv","qgz","vhh","bhc","ptag","ymkb","kh","tbji","vd","wah","wfd","jc","wn","phby","bcm","hcb","jyn","fxm","tj","cwp","cd","hwdw","gdcb","nc","qkb","hbfv","xg","ghn","wykf","fh","xbty","awz","iy","wcvj","zb","qc","ay","fx","wpfy","abxm","fbb","xa","whpd","yh","bwiq","bnz","qx","bya","iyap","knb","fg","hjb","xazd","ghm","hihb","wc","kbi","bc","hw","qbn","jpbz","ki","bz","kdbc","ac","mw","zpv","yxpf","yc","wbmc","xdh","id","qb","bbf","hb","vw","gj","dy","bh","bvb","pw","px","hwbb","vz","my","cg","aith","pqmv","gx","bb","cxfq","yxmc","dczy","jawh","gky","kcy","yacm","kwca","btx","hi","cip","bfcv","mykz","xcb","hw","yic","qzi","cjih","md","bccw","kp","zkyq","qhym","twp","pvc","mbn","bpb","ygpk","hn","vq","jym","yhk","fxc","fvky","kwnw","hbag","mb","jh","xytd","fi","vnqc","yjzv","fmhv","wp","cbpq","wgc","cf","mb","fpt","pw","zha","zajc","mbq","hp","cwp","mww","cmt","bbk","fmgv","yy","bh","tg","zy","ynfy","nbx","tdy","cw","ydac","tf","cny","wcf","xp","nmxt","jyk","yw","ibxj","yth","bh","xc","an","iyw","mhad","zbwm","gahp","hy","dxhj","hwc","cq","mihw","nyvw","ty","dwh","bc","xpg","aij","yf","im","vb","tzc","hgyj","id","gn","bg","vng","wxyz","zw","xci","tyv","kgv","cb","caj","bpvy","qa","nx","hawb","ady","mg","wh","ztm","wph","nx","gqzt","id","cbjm","fbi","zcf","dvjk","azf","qyv","tq","vh","fvyw","dnyg","pjvy","kdw","zfx","bch","agd","jnmd","ad","bfx","zjbd","aw","tvg","vz","za","yf","qy","vmq","cwb","vpg","pawy","pqbw","bw","wmt","bjt","hvcg","ykc","gb","idm","xp","yh","cwxb","fb","fj","bt","hfm","pbhf","ny","aw","jgm","px","wat","fd","pw","cbb","ydkc","qh","bbc","xgi","xmbb","xm","gqb","aw","yzkd","tmc","cta","wbyp","mbbk","wnm","hadv","vgpf","tzqx","cpy","wqgc","ymw","njvd","wy","zbyh","vdyc","cnp","kcgf","dx","ht","gawq","jkb","pww","gk","tbab","bbad","cbwh","my","hdw","xm","bvy","yx","byp","dag","vghm","bvic","hc","vnq","bpy","cmtv","hzkw","ihkf","qvyz","ypmt","qad","qmh","vd","kan","ygw","wnm","xtwq","fzc","jp","yk","bnw","wcw","ajzc","dyiq","yhgw","xidy","wbbi","ccy","wi","ga","bb","gmk","fj","gwbi","wpb","wx","ca","yc","nq","hfxy","bqkb","wihz","bbvc","hg","qtj","dnw","fqh","cg","bc","tc","dp","xcc","nmpb","tg","yb","nv","bt","xmz","ptwa","cbp","xb","hti","cm","nvt","fyw","tyhw","wc","ww","cbb","gky","ihwz","bix","dycn","ht","cyxd","hqyn","ybig","pz","bd","xkfg","qk","hzq","qyw","ybyg","wbj","ab","kpqn","ay","ct","ct","wdfb","aymd","vja","yv","cbcm","kw","hwnk","xnb","fwda","mi","pc","vaww","hzkc","bb","fvb","ykc","vpb","ayti","xdqf","jfcq","gb","bn","gpa","cj","akiq","bp","gyc","hv","dy","icw","ybx","bj","qzc","xzby","bthj","jck","gw","jy","bfwj","cy","hwp","hb","vc","ng","patn","cajc","vhpw","kmbd","dthy","ybi","nv","awb","hpab","xdq","bct","dnh","bxnh","bb","hbb","igyd","vx","wgv","ign","qb","wbih","axic","tgf","zdvw","yb","ah","zjbh","iwyb","gdvw","iwf","dq","wyy","ihdk","qcx","wh","vcft","ny","byhj","bb","yych","wbky","hyw","zcdt","hbh","mwcq","hamk","xat","wib","kwb","yiq","ynt","td","kyw","caw","bn","dgy","hkb","wbx","mjq","vy","mf","kw","bhw","cawg","qgc","jk","bgn","wixa","ybh","zm","wbj","bhhc","hxpy","nh","hi","dk","am","khmf","wdan","wafg","bh","yytj","qhgw","wkt","wz","fay","byyk","hn","wwz","qpz","npb","xw","zpc","jdm","gd","xhcy","bgdp","fnx","kw","yjfw","wxc","kc","awg","zpb","qpdh","vb","gc","bbc","cpbi","bb","vnxb","bq","cbhw","yc","zqdc","aiwy","xjn","fmcb","bd","tgb","bp","hf","afd","iach","bftn","pji","yqm","nf","an","ykwm","izy","yn","fjy","jw","hj","cb","wghi","wp","tzba","twy","kfgt","fcy","npmk","kcit","mcqx","vqt","wb","cgwt","ztw","byp","hcd","bdhp","pxm","bycw","zjfh","kaj","zyg","jgcb","cta","dw","bby","fxzb","hwc","mt","qcz","zn","bwch","tik","yc","cha","jw","ibn","bh","bh","ayg","ywm","xvbc","dcjk","bwx","piw","dxcf","cn","wp","vpc","jw","wgh","hym","tgc","xtw","cg","hy","byc","akf","icj","yxb","ahyv","hcn","bh","akv","cy","jnc","awm","wkc","wcpm","wn","hq","tpb","yc","vcm","ta","vcwg","mbwq","knw","gij","zdyi","ybi","nwt","bfby","pbac","jq","kdy","bhd","kj","qbcj","qwmg","ihnb","hyty","jy","bv","cb","hcvz","zbfv","bgw","yv","zqth","wca","bmc","dfby","nt","in","ji","byj","bjz","vqmb","wb","ztd","ghky","xbg","wcb","bpwx","gy","czbx","jy","zn","bn","wzy","nyf","hvhg","hg","jah","wcb","dqf","cjb","bft","fa","ac","qpyw","ny","ypc","bih","bbw","nbmi","qbd","byfp","bb","ja","ichv","bn","wbdp","cpwc","hzid","jvg","vcqf","by","qmb","kbxd","wn","cdk","wqng","cpiv","gwc","dq","bbn","hgmk","fb","bvpg","wtkj","cbpw","pc","bkw","zh","xiy","whb","vcg","yw","cb","hhf","qhk","qbcp","djv","yqic","hv","yhbp","zpc","yy","wm","nxc","zcmw","af","zb","ck","dv","agin","gwyb","dxnc","gwm","hq","hb","kd","pahf","kn","wcz","wb","iygb","yhng","mx","pjzi","vy","tjb","bvai","gih","wgvh","kbzq","cpzy","yy","tbqm","ndz","twq","wj","mfb","kt","jkxd","dbp","zh","bij","yjqi","bwph","wyf","ybw","qcw","jyd","kp","tiwq","kd","hvhy","xbvy","xg","hw","pq","vjfb","xcib","vfgh","ywvn","ptqa","hqv","bz","pti","ykf","tj","wi","ph","ip","wi","bvw","wbh","pdy","ci","jt","ihyp","gqd","kfzw","db","cpab","ywf","tz","wwby","tnb","bwh","kpdt","bxyb","fjn","cjt","bhy","bv","mbfi","hq","zcah","jxmh","phbn","cfh","yz","yiqw","xhmt","yh","hp","zyji","yqjf","babp","fy","xq","nq","diw","kih","hxb","bnd","imw","wcak","bcwv","qy","txib","jxic","ycpj","icb","cq","mbp","thc","dck","zqa","chdt","bk","wcd","kvhz","tkh","yk","zxhv","yky","fhnx","pyqb","cmyw","xby","cji","amfq","fbtz","bb","yz","hnd","qxic","wn","nbdc","jwcb","nwp","bpb","wbty","bnj","hbg","bw","bkh","hfhx","fynv","ty","bbx","mdgi","yc","bdc","zwnm","bwx","qwht","htg","gy","cgm","xqb","bt","inyf","cah","tamg","kbvh","zj","vfc","wh","xht","ynbc","jpqf","xky","bj","tc","bcx","hqy","xc","jaq","ny","qhzd","pxj","zdc","thwx","czcd","njfc","qdzb","dhcf","xq","dj","wyva","cft","vybj","vt","xcv","gmh","mgyy","bv","mtqc","xhp","gbw","ywmb","yaz","gfqj","nbht","ycmh","whzv","iny","vj","gwb","ayy","myb","cy","xtmc","qc","nx","dy","wyxh","vxjd","tyzn","bpw","kcf","mka","dab","cv","khqp","ci","tqhx","ma","jchy","mj","vtw","mhf","mwch","cvyw","cba","cpzx","kzch","yw","bh","wxj","cbnw","zg","mck","qw","dij","xb","qb","kyb","tvw","wzk","by","nx","bhym","jdhn","yhf","kjb","jb","kbbb","yhkq","idyn","zdmh","bjp","pxhd","bi","bqh","cvct","ckmx","bjp","bkb","xma","my","bzwg","bjy","bixc","bczj","wnw","igb","jyhc","wf","bw","fxi","pba","nmfp","ca","wdy","cacj","wnh","ahvi","hygm","vhqx","vwb","bfjb","fty","bp","qvyp","pibc","jkyd","btk","bmjt","bmtj","fk","by","avdm","ht","wibw","yhw","hng","hxiw","bchk","dw","bkc","htpc","icj","bc","hdyw","yicb","iccz","gk","bb","dx","gkw","wwba","nctz","jb","tnbm","hng","cq","wgqb","jywq","zq","xh","ndt","kdc","aqp","qgvd","dc","kpai","cj","zvg","abd","gxv","zbwc","mby","cwht","xvnz","yh","miyz","xbpb","hatk","ywp","wbyy","xv","vjib","ynx","ab","cmpj","gx","bx","cwb","hi","va","gj","fbq","vywz","hfcw","cjbz","cpid","dkb","wwhb","abjz","nt","yn","nbwh","dtki","bhcp","yzv","hnvc","wbh","xi","ahf","ndyb","dccy","cvij","mvfd","qb","fk","hfaw","bqcy","bf","mfwz","cy","qaw","nqa","wjz","hg","gb","bidp","qz","tbf","dgh","xwf","cac","vpm","xw","wcp","fndb","khcg","baiw","hyfb","kqt","hct","hdq","bhc","thwd","cy","zgkq","wd","cymz","gj","yb","dt","bqzb","th","qbz","hk","cp","cb","iqbd","bnac","ab","bq","xbh","bhyi","ydc","cb","yci","nbc","pc","mbha","vzg","jz","ivcz","fpyc","kn","dxzc","zdc","fkh","bnhh","wtca","cgbv","xk","ybjp","mtw","tw","qwy","iqx","ix","yb","cwf","hyb","gph","hjv","jhyh","cv","dhi","pccf","qb","jy","qydw","bkcx","mczc","hidn","fyt","bhpy","yzn","jdi","zhgx","gkb","bg","vmp","xv","tkjc","vg","mfy","xbqc","wvt","ityx","mv","bqd","dcb","cdh","hb","bb","jbp","tq","vkp","bgcy","cjq","nbft","ivth","pwv","fpt","ng","wmqa","cwbt","btyc","pd","whjd","kayh","djq","vap","ki","ypx","zy","xcy","tvbb","tn","pxyk","wamg","jfby","qhb","yhb","vzq","gz","bwfa","dcb","fxat","mc","kh","nd","bqtw","hfy","yxm","zgbx","pimh","atj","yz","cxzm","bc","wc","bhf","ywt","bwz","jfv","dbc","yb","qci","amyd","tbb","yn","hn","tmc","awh","iwdm","gi","qg","cdb","pi","dig","zbw","fzqa","fdbk","aqt","km","ki","cbzj","zt","bcim","ibz","gby","hgp","whcx","hg","xzf","ty","ykm","wng","zqcv","khyb","cya","bh","cbyk","wbqg","ipwj","agp","jy","im","gknc","qy","ht","ygwc","ngbh","mn","qthg","yt","ncht","tckx","cyb","xbj","yhb","hmh","hcky","ad","mpg","ywqa","biy","jq","cgdw","fj","ptkh","gn","ybvk","xkmc","ygq","wj","taq","wa","nb","jpv","nhw","dywq","xyhc","dqc","wmq","jvgd","yf","wb","bxic","jkmp","zihh","yqa","myjc","kn","wp","ad","cb","wfpw","td","jnb","wb","jznt","xw","zb","wy","jt","jzw","qb","gyh","fbmd","tkig","qh","wbm","fn","nwb","bt","kchb","hnwy","cgb","wnzj","yb"], 4616)

```

    {'zyyh': 1, 'zyy': 1, 'zyxg': 1, 'zyxc': 1, 'zyww': 1, 'zywj': 1, 'zywc': 1, 'zyn': 1, 'zyji': 1, 'zyj': 1, 'zyik': 1, 'zygd': 1, 'zyg': 1, 'zydh': 1, 'zyd': 1, 'zyc': 2, 'zyby': 1, 'zybh': 1, 'zybb': 1, 'zy': 11, 'zxw': 2, 'zxt': 2, 'zxhv': 1, 'zxg': 1, 'zxcy': 1, 'zxcc': 1, 'zxbt': 1, 'zxb': 1, 'zx': 7, 'zwyq': 1, 'zwyg': 1, 'zwy': 2, 'zwwf': 1, 'zwt': 1, 'zwqy': 1, 'zwqv': 1, 'zwnm': 2, 'zwha': 1, 'zwh': 1, 'zwfy': 1, 'zwf': 1, 'zwdv': 1, 'zwbc': 1, 'zwb': 2, 'zwaf': 1, 'zwac': 1, 'zw': 9, 'zvy': 1, 'zvp': 1, 'zvm': 1, 'zvjt': 1, 'zvg': 1, 'zvbh': 2, 'zv': 6, 'ztyb': 1, 'zty': 2, 'ztw': 1, 'ztvd': 1, 'ztp': 2, 'ztn': 1, 'ztm': 1, 'ztjb': 1, 'zthb': 1, 'ztdh': 1, 'ztd': 1, 'ztb': 1, 'ztax': 1, 'zt': 9, 'zqyb': 2, 'zqy': 2, 'zqva': 1, 'zqtj': 1, 'zqth': 2, 'zqj': 1, 'zqi': 1, 'zqhy': 1, 'zqf': 1, 'zqdc': 2, 'zqcv': 1, 'zqcp': 1, 'zqc': 1, 'zqac': 1, 'zqa': 1, 'zq': 7, 'zpyh': 1, 'zpv': 1, 'zpt': 1, 'zpk': 1, 'zpc': 3, 'zpb': 1, 'zp': 8, 'znyy': 1, 'znxd': 1, 'znw': 2, 'znth': 1, 'znmj': 1, 'znmh': 1, 'znk': 1, 'znjd': 1, 'znhy': 1, 'znhw': 1, 'znha': 1, 'znga': 1, 'znf': 1, 'znb': 1, 'znat': 1, 'zn': 10, 'zmx': 1, 'zmw': 1, 'zmvk': 1, 'zmjn': 1, 'zmja': 1, 'zmgw': 1, 'zmfn': 1, 'zmf': 1, 'zmck': 1, 'zm': 7, 'zkyq': 1, 'zkx': 1, 'zkvj': 1, 'zkv': 1, 'zkt': 1, 'zkq': 1, 'zknb': 1, 'zkm': 1, 'zkht': 1, 'zkg': 1, 'zkca': 1, 'zkc': 2, 'zkb': 1, 'zka': 2, 'zk': 8, 'zjyc': 1, 'zjyb': 1, 'zjw': 1, 'zjv': 1, 'zjtf': 1, 'zjn': 1, 'zjkv': 1, 'zjic': 1, 'zjg': 1, 'zjfh': 1, 'zjca': 1, 'zjbh': 1, 'zjbg': 1, 'zjbd': 1, 'zj': 4, 'ziyn': 1, 'ziy': 1, 'ziv': 1, 'zip': 2, 'zim': 1, 'zihh': 1, 'zih': 1, 'zifq': 1, 'zibh': 1, 'zia': 1, 'zi': 4, 'zhyg': 1, 'zhy': 1, 'zhwn': 1, 'zhwa': 1, 'zhw': 5, 'zhvy': 1, 'zhvt': 1, 'zhv': 1, 'zhpj': 1, 'zhp': 1, 'zhjb': 1, 'zhj': 1, 'zhhx': 1, 'zhgx': 1, 'zhf': 1, 'zhdi': 1, 'zhd': 1, 'zhbj': 1, 'zhb': 2, 'zhay': 1, 'zhaq': 1, 'zha': 1, 'zh': 18, 'zgw': 1, 'zgt': 1, 'zgq': 1, 'zgn': 1, 'zgmk': 1, 'zgkq': 1, 'zgk': 1, 'zgi': 1, 'zght': 1, 'zgf': 1, 'zgbx': 1, 'zga': 1, 'zg': 5, 'zfx': 1, 'zfw': 1, 'zfva': 1, 'zfv': 2, 'zfpx': 1, 'zfp': 1, 'zfk': 1, 'zfja': 1, 'zfi': 1, 'zfhn': 1, 'zfg': 1, 'zfdx': 1, 'zfd': 1, 'zfc': 1, 'zfb': 2, 'zf': 2, 'zdyi': 1, 'zdyg': 1, 'zdx': 1, 'zdwc': 1, 'zdvw': 1, 'zdn': 1, 'zdmp': 1, 'zdmh': 1, 'zdi': 1, 'zdht': 1, 'zdfc': 1, 'zdf': 1, 'zdci': 1, 'zdc': 3, 'zdbh': 1, 'zdb': 1, 'zda': 1, 'zd': 5, 'zcyn': 1, 'zcyh': 1, 'zcw': 1, 'zcvp': 1, 'zcmy': 1, 'zcmw': 1, 'zcm': 1, 'zck': 1, 'zci': 1, 'zchx': 1, 'zchh': 1, 'zcga': 1, 'zcfy': 1, 'zcf': 1, 'zcdy': 1, 'zcdt': 1, 'zcdh': 1, 'zcd': 1, 'zcc': 1, 'zcbw': 1, 'zcb': 6, 'zcah': 1, 'zc': 14, 'zbyx': 1, 'zbyh': 1, 'zbwn': 1, 'zbwm': 1, 'zbwc': 1, 'zbw': 1, 'zbv': 1, 'zbtw': 1, 'zbqy': 1, 'zbpy': 1, 'zbpq': 1, 'zbp': 1, 'zbn': 2, 'zbkq': 1, 'zbkb': 1, 'zbk': 1, 'zbj': 1, 'zbiw': 1, 'zbib': 1, 'zbi': 1, 'zbh': 1, 'zbfv': 1, 'zbdq': 1, 'zbd': 1, 'zbbv': 1, 'zbaf': 1, 'zb': 16, 'zayh': 1, 'zay': 1, 'zav': 1, 'zat': 1, 'zamw': 1, 'zajc': 1, 'zaiw': 1, 'zahw': 1, 'zahh': 1, 'zafh': 1, 'zabp': 1, 'zabi': 1, 'za': 13, 'yzyw': 1, 'yzwb': 1, 'yzw': 1, 'yzv': 1, 'yzq': 1, 'yznb': 2, 'yzn': 2, 'yzmd': 1, 'yzm': 1, 'yzkd': 1, 'yzj': 2, 'yzi': 1, 'yzhy': 1, 'yzh': 1, 'yzf': 1, 'yzdx': 1, 'yzdq': 1, 'yzcn': 1, 'yzcb': 1, 'yzbb': 1, 'yzb': 1, 'yz': 20, 'yywg': 1, 'yyw': 2, 'yyv': 1, 'yytj': 1, 'yyth': 1, 'yyp': 1, 'yyn': 1, 'yyhc': 1, 'yyg': 1, 'yyft': 1, 'yyfk': 2, 'yych': 3, 'yyc': 1, 'yybt': 1, 'yybp': 1, 'yybf': 1, 'yybb': 1, 'yyba': 1, 'yyb': 1, 'yya': 1, 'yy': 12, 'yxz': 2, 'yxya': 1, 'yxw': 1, 'yxvj': 1, 'yxpf': 1, 'yxpb': 1, 'yxp': 1, 'yxnj': 1, 'yxmc': 1, 'yxm': 1, 'yxja': 1, 'yxj': 1, 'yxi': 3, 'yxh': 1, 'yxg': 1, 'yxfv': 1, 'yxf': 1, 'yxd': 1, 'yxc': 1, 'yxbq': 1, 'yxbn': 1, 'yxbh': 1, 'yxbb': 1, 'yxb': 1, 'yxa': 1, 'yx': 6, 'ywz': 2, 'ywyc': 1, 'ywy': 1, 'ywxg': 1, 'ywxb': 1, 'ywx': 2, 'ywwb': 1, 'yww': 1, 'ywvn': 1, 'ywv': 2, 'ywtb': 1, 'ywt': 1, 'ywqg': 1, 'ywqb': 1, 'ywqa': 1, 'ywq': 2, 'ywpn': 1, 'ywp': 2, 'ywny': 1, 'ywnh': 1, 'ywnb': 1, 'ywmx': 1, 'ywmb': 1, 'ywm': 1, 'ywkb': 1, 'ywk': 2, 'ywjh': 1, 'ywiy': 1, 'ywiq': 1, 'ywi': 1, 'ywh': 1, 'ywg': 1, 'ywfq': 1, 'ywfg': 1, 'ywfa': 1, 'ywf': 2, 'ywc': 1, 'ywbk': 2, 'ywbb': 1, 'ywb': 1, 'ywaw': 1, 'ywak': 1, 'yw': 32, 'yvx': 1, 'yvwk': 1, 'yvwc': 1, 'yvwb': 1, 'yvw': 1, 'yvq': 1, 'yvph': 1, 'yvp': 1, 'yvi': 1, 'yvhp': 1, 'yvfg': 1, 'yvdn': 1, 'yvb': 1, 'yvak': 1, 'yv': 15, 'ytxh': 1, 'ytwy': 1, 'ytww': 1, 'ytwk': 1, 'ytw': 2, 'ytv': 2, 'ytqp': 1, 'ytq': 2, 'ytp': 1, 'ytmn': 1, 'ytm': 1, 'ytkw': 1, 'ytkh': 1, 'ytji': 1, 'ytj': 1, 'ytib': 1, 'ythw': 1, 'ythc': 1, 'yth': 1, 'ytgz': 1, 'ytg': 1, 'ytfk': 1, 'ytf': 1, 'ytd': 1, 'ytcm': 1, 'ytc': 1, 'ytb': 1, 'yt': 10, 'yqz': 1, 'yqy': 2, 'yqw': 1, 'yqvi': 1, 'yqvh': 1, 'yqv': 1, 'yqtg': 1, 'yqpz': 1, 'yqpj': 1, 'yqp': 1, 'yqm': 1, 'yqk': 1, 'yqjf': 1, 'yqic': 1, 'yqfy': 1, 'yqfi': 1, 'yqf': 2, 'yqc': 1, 'yqah': 1, 'yqa': 1, 'yq': 15, 'ypz': 1, 'ypxj': 1, 'ypx': 1, 'ypvw': 1, 'ypvb': 1, 'ypnb': 1, 'ypmt': 1, 'ypm': 1, 'ypiy': 1, 'ypiv': 1, 'ypi': 1, 'ypht': 1, 'ypg': 1, 'ypfb': 1, 'ypc': 1, 'yp': 11, 'ynzb': 1, 'ynyz': 1, 'ynxb': 1, 'ynx': 1, 'ynwc': 1, 'ynwa': 1, 'ynw': 2, 'ynt': 2, 'ynq': 1, 'ynmz': 1, 'ynk': 1, 'ynim': 1, 'yni': 1, 'ynhd': 1, 'ynh': 1, 'yngq': 2, 'yngd': 1, 'ynfy': 1, 'ynf': 1, 'yndb': 1, 'yncx': 1, 'yncm': 1, 'ync': 2, 'ynbi': 1, 'ynbg': 1, 'ynbc': 1, 'ynaw': 1, 'ynav': 1, 'yn': 22, 'ymzc': 1, 'ymy': 1, 'ymwd': 1, 'ymw': 2, 'ymv': 1, 'ymtc': 1, 'ymqz': 1, 'ymqb': 1, 'ymq': 2, 'ympb': 1, 'ymkb': 1, 'ymjd': 1, 'ymh': 1, 'ymgw': 1, 'ymf': 1, 'ymd': 1, 'ym': 10, 'yky': 1, 'ykxj': 1, 'ykwq': 1, 'ykwm': 1, 'ykwg': 1, 'ykw': 1, 'ykv': 1, 'ykq': 1, 'ykm': 2, 'ykhv': 1, 'ykht': 1, 'ykh': 1, 'ykg': 1, 'ykfn': 1, 'ykf': 2, 'ykd': 1, 'ykcz': 1, 'ykcf': 1, 'ykc': 3, 'ykb': 1, 'ykan': 1, 'yk': 8, 'yjzv': 1, 'yjyd': 1, 'yjy': 1, 'yjxq': 1, 'yjx': 2, 'yjwb': 1, 'yjwa': 1, 'yjw': 3, 'yjtd': 1, 'yjqi': 1, 'yjq': 2, 'yjpb': 1, 'yjmk': 1, 'yjmh': 1, 'yjic': 1, 'yjhb': 1, 'yjgt': 1, 'yjg': 1, 'yjfw': 1, 'yjft': 1, 'yjfb': 1, 'yjch': 1, 'yjbc': 1, 'yj': 13, 'yiyk': 1, 'yixa': 1, 'yiqw': 1, 'yiqj': 1, 'yiqb': 1, 'yiq': 1, 'yinc': 1, 'yimg': 1, 'yik': 3, 'yij': 1, 'yifh': 1, 'yif': 2, 'yicb': 1, 'yic': 2, 'yib': 2, 'yia': 1, 'yi': 15, 'yhy': 1, 'yhxk': 1, 'yhxg': 1, 'yhx': 1, 'yhw': 2, 'yhvk': 1, 'yhvh': 1, 'yhv': 1, 'yhtb': 1, 'yht': 1, 'yhqz': 1, 'yhqc': 1, 'yhp': 1, 'yhng': 2, 'yhma': 1, 'yhm': 1, 'yhkq': 1, 'yhk': 1, 'yhjq': 1, 'yhj': 1, 'yhhz': 1, 'yhhb': 1, 'yhgx': 1, 'yhgw': 1, 'yhgj': 1, 'yhfc': 1, 'yhf': 1, 'yhcz': 1, 'yhcw': 1, 'yhc': 1, 'yhbp': 1, 'yhbh': 1, 'yhba': 1, 'yhb': 3, 'yh': 19, 'ygyw': 1, 'ygyn': 1, 'ygy': 1, 'ygwh': 1, 'ygwc': 1, 'ygwb': 1, 'ygw': 2, 'ygv': 2, 'ygqz': 1, 'ygqd': 1, 'ygq': 2, 'ygpk': 1, 'ygp': 1, 'ygmq': 1, 'ygm': 1, 'ygkf': 1, 'ygid': 1, 'ygi': 1, 'yghd': 1, 'ygh': 1, 'ygf': 1, 'ygdw': 1, 'ygc': 1, 'ygbz': 1, 'ygaz': 1, 'yg': 8, 'yfzw': 1, 'yfzk': 1, 'yfyw': 1, 'yfx': 1, 'yfw': 1, 'yfic': 1, 'yfhp': 1, 'yfh': 1, 'yfgw': 1, 'yfgm': 1, 'yfg': 1, 'yfcm': 1, 'yfc': 1, 'yfbw': 1, 'yfb': 1, 'yfa': 2, 'yf': 12, 'ydyw': 1, 'ydyc': 1, 'ydv': 1, 'ydpc': 1, 'ydpb': 1, 'ydp': 1, 'ydm': 1, 'ydkc': 1, 'ydj': 1, 'ydi': 1, 'ydh': 3, 'ydfn': 1, 'ydfj': 1, 'ydfh': 1, 'ydf': 1, 'ydcq': 1, 'ydcb': 2, 'ydc': 1, 'ydbi': 1, 'ydbh': 1, 'ydam': 1, 'ydac': 1, 'ydab': 1, 'yd': 12, 'ycya': 1, 'ycy': 1, 'ycx': 1, 'ycwb': 1, 'ycvz': 1, 'ycvq': 1, 'ycv': 1, 'ycqz': 1, 'ycpj': 1, 'ycn': 3, 'ycmh': 1, 'ycmg': 1, 'ycm': 2, 'ycji': 1, 'ycjb': 1, 'ycj': 3, 'yciw': 1, 'ycic': 1, 'yci': 1, 'ychw': 1, 'ychc': 1, 'ycgz': 1, 'ycgx': 1, 'ycga': 1, 'ycg': 1, 'ycf': 2, 'ycdc': 1, 'ycc': 1, 'ycbn': 1, 'ycbd': 1, 'ycba': 1, 'ycb': 1, 'ycap': 1, 'yc': 29, 'ybzn': 1, 'ybz': 2, 'ybyt': 1, 'ybyg': 1, 'yby': 2, 'ybxy': 1, 'ybxp': 1, 'ybx': 4, 'ybwy': 1, 'ybwv': 1, 'ybwp': 1, 'ybwj': 1, 'ybwb': 1, 'ybwa': 1, 'ybw': 4, 'ybvk': 1, 'ybqz': 1, 'ybqc': 1, 'ybq': 1, 'ybpz': 1, 'ybpw': 1, 'ybpn': 1, 'ybpk': 1, 'ybp': 2, 'ybnw': 1, 'ybnc': 2, 'ybn': 1, 'ybmq': 1, 'ybm': 2, 'ybkw': 1, 'ybkp': 1, 'ybkf': 1, 'ybkc': 1, 'ybjp': 1, 'ybjm': 1, 'ybj': 4, 'ybig': 1, 'ybi': 4, 'ybhm': 1, 'ybhd': 1, 'ybh': 6, 'ybgc': 1, 'ybgb': 1, 'ybfw': 1, 'ybf': 1, 'ybd': 2, 'ybcx': 1, 'ybcw': 1, 'ybct': 1, 'ybcn': 1, 'ybch': 1, 'ybcb': 1, 'ybc': 2, 'ybbz': 1, 'ybbv': 2, 'ybb': 1, 'ybab': 1, 'yba': 1, 'yb': 41, 'yaz': 3, 'yayb': 2, 'yay': 1, 'yaxw': 1, 'yawk': 1, 'yavd': 1, 'yap': 1, 'yan': 1, 'yamy': 1, 'yakn': 1, 'yah': 1, 'yag': 2, 'yafb': 1, 'yadw': 1, 'yacm': 1, 'yabh': 1, 'ya': 8, 'xzyd': 1, 'xzy': 2, 'xzwj': 1, 'xzt': 1, 'xzq': 1, 'xzm': 1, 'xzjb': 1, 'xzj': 2, 'xzf': 1, 'xzd': 1, 'xzby': 1, 'xzbb': 1, 'xzb': 1, 'xza': 1, 'xz': 5, 'xyzb': 1, 'xyz': 1, 'xyvw': 1, 'xytn': 1, 'xytd': 1, 'xyp': 1, 'xyn': 1, 'xyib': 1, 'xyi': 1, 'xyhc': 1, 'xyh': 2, 'xygt': 1, 'xygf': 1, 'xyf': 1, 'xycd': 1, 'xyby': 1, 'xybw': 1, 'xyb': 4, 'xyat': 1, 'xy': 9, 'xwza': 1, 'xww': 1, 'xwqt': 1, 'xwpz': 1, 'xwn': 1, 'xwmw': 1, 'xwmk': 1, 'xwm': 1, 'xwkz': 1, 'xwkg': 1, 'xwk': 2, 'xwh': 2, 'xwfy': 1, 'xwfj': 1, 'xwfh': 1, 'xwf': 1, 'xwdb': 1, 'xwcc': 1, 'xwbp': 1, 'xwbn': 1, 'xw': 11, 'xvwq': 1, 'xvqt': 1, 'xvpi': 1, 'xvp': 1, 'xvnz': 1, 'xvjc': 1, 'xvj': 1, 'xvd': 1, 'xvc': 1, 'xvbj': 1, 'xvbc': 2, 'xva': 1, 'xv': 4, 'xty': 1, 'xtwq': 1, 'xtw': 1, 'xtna': 1, 'xtn': 1, 'xtmc': 1, 'xti': 1, 'xthk': 1, 'xth': 1, 'xtb': 2, 'xt': 9, 'xqzh': 1, 'xqzf': 1, 'xqzc': 1, 'xqwc': 1, 'xqvi': 1, 'xqjy': 1, 'xqf': 1, 'xqcw': 1, 'xqc': 1, 'xqbv': 1, 'xqbp': 1, 'xqb': 3, 'xqat': 1, 'xqab': 1, 'xq': 5, 'xpy': 1, 'xpty': 1, 'xpq': 1, 'xpn': 1, 'xphv': 1, 'xph': 1, 'xpg': 1, 'xpc': 1, 'xp': 5, 'xngv': 1, 'xndi': 1, 'xnd': 1, 'xnc': 1, 'xnb': 3, 'xn': 5, 'xmzj': 1, 'xmz': 1, 'xmyj': 1, 'xmwd': 1, 'xmwb': 1, 'xmw': 1, 'xmqt': 1, 'xmk': 1, 'xmj': 1, 'xmiw': 1, 'xmi': 1, 'xmh': 1, 'xmd': 2, 'xmbk': 1, 'xmbf': 1, 'xmbb': 1, 'xmb': 1, 'xma': 1, 'xm': 7, 'xkya': 1, 'xky': 1, 'xkwh': 1, 'xkt': 1, 'xkqy': 1, 'xkp': 1, 'xknb': 1, 'xkmj': 1, 'xkmc': 1, 'xkfn': 1, 'xkfg': 1, 'xkbi': 1, 'xkb': 1, 'xk': 8, 'xjv': 1, 'xjqm': 1, 'xjpw': 1, 'xjp': 1, 'xjn': 1, 'xjig': 1, 'xjhy': 1, 'xjb': 1, 'xj': 2, 'xiz': 1, 'xiyt': 1, 'xiy': 1, 'xiw': 1, 'xivt': 1, 'xit': 2, 'xiq': 1, 'xip': 1, 'xinw': 1, 'xik': 1, 'xijn': 1, 'xihg': 1, 'xih': 2, 'xig': 1, 'xidy': 1, 'xicb': 1, 'xibh': 1, 'xibg': 1, 'xib': 1, 'xi': 5, 'xhz': 1, 'xhyv': 1, 'xhy': 1, 'xhw': 3, 'xhv': 1, 'xhtg': 1, 'xht': 1, 'xhq': 1, 'xhpb': 1, 'xhp': 2, 'xhny': 1, 'xhna': 1, 'xhmt': 1, 'xhm': 1, 'xhkz': 1, 'xhk': 1, 'xhj': 1, 'xhiq': 1, 'xhia': 1, 'xhi': 1, 'xhhw': 1, 'xhhp': 1, 'xhdk': 1, 'xhcy': 1, 'xhcb': 1, 'xhba': 1, 'xhb': 1, 'xhac': 1, 'xh': 12, 'xgym': 1, 'xgw': 1, 'xgvt': 1, 'xgv': 1, 'xgq': 1, 'xgm': 1, 'xgi': 1, 'xghk': 1, 'xgh': 1, 'xgfy': 1, 'xgcy': 1, 'xgbw': 1, 'xgbb': 1, 'xg': 10, 'xfz': 1, 'xfqk': 1, 'xfna': 1, 'xfn': 2, 'xfky': 1, 'xfj': 1, 'xfc': 1, 'xf': 4, 'xdqf': 1, 'xdq': 1, 'xdp': 1, 'xdk': 1, 'xdhj': 1, 'xdh': 1, 'xdcq': 1, 'xdbi': 1, 'xdb': 1, 'xd': 5, 'xcyi': 1, 'xcy': 1, 'xcw': 1, 'xcv': 2, 'xcta': 1, 'xcq': 1, 'xcng': 2, 'xcnb': 1, 'xcmz': 1, 'xckc': 1, 'xcjw': 1, 'xcjp': 1, 'xcjc': 1, 'xcj': 2, 'xcib': 1, 'xci': 1, 'xchy': 1, 'xchq': 1, 'xchc': 1, 'xcdy': 1, 'xcd': 2, 'xcc': 1, 'xcbj': 1, 'xcbg': 1, 'xcb': 3, 'xc': 18, 'xbzt': 1, 'xbyt': 1, 'xby': 3, 'xbwg': 1, 'xbw': 1, 'xbvy': 1, 'xbv': 1, 'xbty': 1, 'xbqh': 1, 'xbqc': 2, 'xbq': 3, 'xbpg': 1, 'xbpb': 1, 'xbnz': 1, 'xbnc': 1, 'xbn': 1, 'xbji': 1, 'xbj': 2, 'xbi': 1, 'xbhf': 1, 'xbhd': 1, 'xbh': 2, 'xbg': 1, 'xbf': 1, 'xbdw': 1, 'xbcw': 1, 'xbc': 2, 'xbbv': 1, 'xbbk': 1, 'xbb': 1, 'xbap': 1, 'xbam': 1, 'xb': 18, 'xazd': 1, 'xayw': 1, 'xay': 2, 'xaw': 1, 'xavy': 1, 'xat': 1, 'xan': 1, 'xai': 1, 'xag': 1, 'xaf': 1, 'xac': 1, 'xa': 6, 'wzy': 1, 'wzxt': 1, 'wzvh': 1, 'wzq': 2, 'wzpq': 1, 'wzp': 1, 'wzmt': 1, 'wzmn': 1, 'wzm': 1, 'wzk': 2, 'wzj': 1, 'wzij': 1, 'wzi': 1, 'wzhy': 1, 'wzh': 1, 'wzf': 1, 'wzd': 1, 'wzcq': 1, 'wzcm': 1, 'wzcb': 1, 'wzc': 1, 'wzb': 2, 'wzax': 1, 'wz': 15, 'wyzq': 1, 'wyyt': 1, 'wyy': 1, 'wyxh': 2, 'wyxb': 1, 'wywa': 1, 'wyw': 3, 'wyva': 1, 'wyv': 1, 'wyt': 1, 'wyqz': 1, 'wyqk': 1, 'wyqh': 2, 'wyq': 2, 'wyp': 1, 'wynh': 2, 'wym': 3, 'wykf': 1, 'wyk': 2, 'wyjn': 1, 'wyjg': 1, 'wyj': 1, 'wyic': 1, 'wyi': 1, 'wyhy': 1, 'wyh': 1, 'wygc': 2, 'wygb': 1, 'wyfv': 1, 'wyf': 1, 'wydw': 1, 'wydb': 1, 'wycn': 1, 'wycm': 1, 'wyca': 1, 'wyc': 1, 'wybh': 1, 'wyb': 5, 'wya': 1, 'wy': 23, 'wxyz': 1, 'wxyw': 1, 'wxy': 1, 'wxwc': 1, 'wxmd': 1, 'wxj': 1, 'wxgq': 2, 'wxfb': 1, 'wxfa': 1, 'wxca': 1, 'wxc': 1, 'wxb': 1, 'wxaz': 1, 'wxa': 2, 'wx': 18, 'wwz': 1, 'wwym': 1, 'wwx': 1, 'wwv': 1, 'wwth': 1, 'wwp': 1, 'wwn': 1, 'wwkb': 1, 'wwik': 1, 'wwi': 1, 'wwhb': 1, 'wwgk': 1, 'wwdc': 1, 'wwch': 1, 'wwcc': 1, 'wwc': 1, 'wwby': 1, 'wwba': 1, 'wwb': 1, 'ww': 13, 'wvyz': 1, 'wvyy': 1, 'wvt': 1, 'wvqh': 1, 'wvq': 1, 'wvn': 2, 'wvhf': 1, 'wvh': 3, 'wvg': 1, 'wvc': 2, 'wvba': 1, 'wvb': 1, 'wv': 9, 'wtyk': 1, 'wtyb': 1, 'wtya': 1, 'wtvj': 1, 'wtqx': 1, 'wtqn': 1, 'wtq': 1, 'wtn': 1, 'wtmc': 1, 'wtkj': 1, 'wtk': 1, 'wtj': 2, 'wti': 2, 'wth': 3, 'wtfx': 1, 'wtcn': 1, 'wtcd': 1, 'wtca': 1, 'wtbw': 1, 'wtb': 1, 'wta': 1, 'wt': 11, 'wqzw': 1, 'wqzh': 1, 'wqz': 1, 'wqy': 1, 'wqt': 1, 'wqp': 1, 'wqng': 1, 'wqn': 1, 'wqmv': 1, 'wqmg': 1, 'wqmf': 1, 'wqkg': 1, 'wqjw': 1, 'wqj': 1, 'wqij': 1, 'wqi': 1, 'wqhk': 1, 'wqh': 2, 'wqgw': 1, 'wqgc': 1, 'wqf': 1, 'wqdn': 1, 'wqbm': 1, 'wqb': 3, 'wq': 11, 'wpz': 2, 'wpy': 2, 'wpxi': 1, 'wpx': 2, 'wpwc': 1, 'wpn': 1, 'wpm': 2, 'wpkg': 1, 'wpjh': 1, 'wpih': 1, 'wphi': 1, 'wphd': 1, 'wphc': 1, 'wph': 1, 'wpgh': 1, 'wpfy': 1, 'wpd': 1, 'wpb': 4, 'wpa': 1, 'wp': 17, 'wnzj': 1, 'wnyx': 1, 'wny': 3, 'wnx': 3, 'wnw': 1, 'wnv': 1, 'wnq': 1, 'wnp': 1, 'wnm': 2, 'wnh': 2, 'wng': 3, 'wncb': 1, 'wnc': 4, 'wnbv': 1, 'wnb': 1, 'wn': 12, 'wmyp': 1, 'wmy': 1, 'wmx': 1, 'wmwh': 1, 'wmty': 1, 'wmt': 2, 'wmqa': 1, 'wmq': 1, 'wmp': 1, 'wmnb': 1, 'wmkf': 1, 'wmji': 1, 'wmj': 1, 'wmh': 1, 'wmg': 2, 'wmfg': 1, 'wmf': 1, 'wmd': 1, 'wmc': 1, 'wmbg': 1, 'wmaq': 1, 'wm': 14, 'wkyb': 1, 'wky': 1, 'wkx': 1, 'wkt': 2, 'wkp': 1, 'wknf': 1, 'wkn': 1, 'wkj': 3, 'wki': 1, 'wkhb': 1, 'wkh': 1, 'wkgq': 1, 'wkgd': 1, 'wkg': 2, 'wkfw': 1, 'wkfp': 1, 'wkf': 1, 'wkdf': 1, 'wkcm': 1, 'wkcf': 1, 'wkc': 3, 'wkbc': 1, 'wkb': 1, 'wkah': 1, 'wk': 11, 'wjzb': 1, 'wjz': 1, 'wjyw': 1, 'wjyc': 1, 'wjy': 1, 'wjv': 1, 'wjkh': 1, 'wjhb': 1, 'wjg': 1, 'wjfc': 1, 'wjf': 1, 'wjdy': 1, 'wjb': 1, 'wj': 11, 'wizt': 1, 'wiy': 1, 'wixa': 1, 'wix': 1, 'wipy': 1, 'wipf': 1, 'win': 2, 'wij': 1, 'wihz': 1, 'wihy': 1, 'wihn': 1, 'wify': 1, 'widk': 1, 'wid': 1, 'wicw': 1, 'wibw': 1, 'wibv': 1, 'wib': 3, 'wi': 11, 'whzv': 1, 'whzt': 1, 'whyg': 1, 'why': 1, 'whx': 1, 'whw': 1, 'whvd': 1, 'wht': 3, 'whqv': 1, 'whqc': 1, 'whq': 1, 'whpt': 1, 'whpd': 1, 'whp': 1, 'whnw': 1, 'whnp': 1, 'whnh': 1, 'whkn': 1, 'whkg': 1, 'whk': 1, 'whjd': 2, 'whj': 2, 'whiv': 1, 'whij': 1, 'whi': 1, 'whhy': 1, 'whhn': 1, 'whh': 3, 'whfy': 2, 'whfv': 1, 'whfq': 1, 'whfa': 1, 'whdq': 1, 'whdm': 1, 'whdh': 1, 'whcx': 1, 'whcn': 1, 'whci': 1, 'whc': 1, 'whb': 4, 'whah': 1, 'wh': 21, 'wgz': 1, 'wgyv': 1, 'wgyn': 1, 'wgyf': 1, 'wgxq': 1, 'wgxh': 1, 'wgxd': 1, 'wgxa': 1, 'wgx': 1, 'wgwj': 1, 'wgwa': 1, 'wgvh': 1, 'wgv': 2, 'wgqb': 1, 'wgp': 1, 'wgn': 2, 'wgkf': 1, 'wgk': 1, 'wgi': 1, 'wghq': 1, 'wghi': 1, 'wgh': 2, 'wgf': 2, 'wgcq': 1, 'wgc': 3, 'wgby': 1, 'wgbw': 1, 'wgbc': 1, 'wgbb': 1, 'wgb': 1, 'wgac': 1, 'wg': 14, 'wfw': 1, 'wfv': 1, 'wftq': 1, 'wft': 1, 'wfpw': 1, 'wfnv': 1, 'wfn': 1, 'wfjm': 1, 'wfjh': 1, 'wfix': 1, 'wfh': 1, 'wfdb': 1, 'wfd': 3, 'wfc': 1, 'wfb': 3, 'wf': 8, 'wdzj': 1, 'wdy': 2, 'wdwc': 1, 'wdv': 1, 'wdq': 1, 'wdp': 1, 'wdny': 1, 'wdnb': 1, 'wdn': 2, 'wdkz': 1, 'wdjz': 1, 'wdjb': 1, 'wdhh': 1, 'wdg': 2, 'wdfb': 1, 'wdc': 2, 'wdbz': 1, 'wdbi': 1, 'wdb': 1, 'wdan': 2, 'wd': 12, 'wcz': 1, 'wcyx': 1, 'wcyh': 1, 'wcyb': 1, 'wcy': 2, 'wcx': 2, 'wcwg': 1, 'wcw': 2, 'wcvj': 1, 'wcv': 2, 'wct': 1, 'wcqz': 1, 'wcpm': 1, 'wcp': 2, 'wcng': 1, 'wcnc': 1, 'wcn': 2, 'wcmn': 1, 'wcm': 1, 'wck': 1, 'wcjx': 1, 'wci': 1, 'wch': 1, 'wcgx': 1, 'wcg': 5, 'wcf': 4, 'wcdn': 1, 'wcd': 2, 'wccj': 1, 'wcc': 1, 'wcbh': 1, 'wcb': 7, 'wcak': 1, 'wcac': 1, 'wca': 3, 'wc': 29, 'wbz': 1, 'wbyy': 1, 'wbyv': 1, 'wbyp': 1, 'wbyf': 1, 'wbxf': 1, 'wbxa': 1, 'wbx': 2, 'wbwp': 1, 'wbw': 2, 'wbvi': 1, 'wbty': 1, 'wbt': 3, 'wbqz': 1, 'wbqg': 1, 'wbq': 2, 'wbp': 1, 'wbnm': 1, 'wbmt': 1, 'wbmc': 1, 'wbmb': 1, 'wbm': 2, 'wbky': 1, 'wbkx': 1, 'wbj': 4, 'wbih': 1, 'wbi': 2, 'wbhj': 1, 'wbhi': 1, 'wbhf': 1, 'wbhc': 1, 'wbh': 4, 'wbg': 2, 'wbfw': 1, 'wbfj': 1, 'wbfc': 1, 'wbf': 2, 'wbdp': 1, 'wbdg': 1, 'wbdf': 1, 'wbdc': 1, 'wbda': 1, 'wbd': 1, 'wbcx': 1, 'wbcd': 1, 'wbcb': 1, 'wbc': 2, 'wbbw': 1, 'wbbm': 1, 'wbbi': 1, 'wbb': 1, 'wbaz': 1, 'wbav': 1, 'wbat': 1, 'wbam': 1, 'wba': 2, 'wb': 41, 'wazk': 1, 'wax': 1, 'waw': 1, 'wavw': 1, 'wav': 2, 'wat': 1, 'wap': 1, 'wamg': 1, 'waky': 1, 'wak': 1, 'waj': 1, 'waih': 1, 'wahv': 1, 'wah': 1, 'wagb': 1, 'wafq': 1, 'wafg': 1, 'wafb': 1, 'wad': 1, 'wacx': 1, 'wabm': 1, 'wabc': 1, 'wab': 3, 'wa': 11, 'vzxb': 1, 'vzwh': 1, 'vzwf': 1, 'vzw': 1, 'vzt': 1, 'vzq': 2, 'vzmb': 1, 'vzjy': 1, 'vzg': 1, 'vzdb': 1, 'vzbb': 1, 'vzb': 1, 'vz': 6, 'vyyh': 1, 'vyx': 1, 'vywz': 1, 'vywc': 1, 'vyw': 3, 'vyjb': 1, 'vyi': 1, 'vyh': 1, 'vygt': 1, 'vygh': 1, 'vyfh': 1, 'vyfc': 1, 'vyf': 1, 'vyd': 1, 'vycc': 1, 'vyby': 1, 'vybj': 1, 'vybh': 1, 'vybd': 1, 'vyb': 3, 'vya': 1, 'vy': 12, 'vxt': 1, 'vxqi': 1, 'vxmc': 1, 'vxm': 1, 'vxk': 1, 'vxjd': 1, 'vxh': 1, 'vxg': 1, 'vxfh': 1, 'vxby': 1, 'vxb': 2, 'vxap': 1, 'vx': 8, 'vwz': 2, 'vwxg': 1, 'vwtj': 1, 'vwt': 1, 'vwpz': 1, 'vwnb': 1, 'vwkg': 1, 'vwkc': 1, 'vwip': 1, 'vwi': 1, 'vwhi': 1, 'vwhd': 1, 'vwh': 2, 'vwf': 1, 'vwck': 1, 'vwb': 2, 'vwa': 1, 'vw': 12, 'vty': 1, 'vtxg': 1, 'vtwh': 1, 'vtw': 1, 'vtqi': 1, 'vtk': 1, 'vth': 1, 'vtbb': 1, 'vtb': 3, 'vt': 9, 'vqz': 1, 'vqxg': 1, 'vqw': 1, 'vqt': 1, 'vqmb': 1, 'vqm': 1, 'vqjy': 1, 'vqhn': 1, 'vqc': 1, 'vq': 5, 'vpqg': 1, 'vpq': 1, 'vpmh': 1, 'vpm': 3, 'vpi': 1, 'vph': 1, 'vpg': 1, 'vpda': 1, 'vpca': 1, 'vpc': 1, 'vpb': 3, 'vp': 5, 'vnxw': 1, 'vnxb': 1, 'vnwz': 1, 'vnqc': 1, 'vnq': 1, 'vnp': 1, 'vnk': 1, 'vnh': 1, 'vng': 2, 'vnfz': 1, 'vnbx': 1, 'vn': 3, 'vmzw': 1, 'vmwz': 1, 'vmt': 1, 'vmq': 1, 'vmpi': 1, 'vmp': 1, 'vmjh': 1, 'vmj': 1, 'vmhw': 1, 'vmbc': 1, 'vma': 1, 'vm': 2, 'vkz': 1, 'vkyh': 1, 'vky': 2, 'vkx': 1, 'vkp': 1, 'vkn': 1, 'vkhi': 1, 'vkh': 1, 'vkfj': 1, 'vkbh': 1, 'vkb': 3, 'vk': 4, 'vjy': 1, 'vjp': 1, 'vjix': 1, 'vjib': 1, 'vjhz': 1, 'vjfb': 1, 'vjbf': 1, 'vja': 1, 'vj': 7, 'viyw': 1, 'viwy': 1, 'viwh': 1, 'viqb': 1, 'vip': 1, 'vimn': 1, 'vik': 1, 'vihy': 1, 'vic': 1, 'viby': 1, 'vi': 4, 'vhzm': 1, 'vhy': 1, 'vhx': 1, 'vhw': 1, 'vhqx': 1, 'vhpw': 2, 'vhk': 1, 'vhhy': 1, 'vhh': 2, 'vhf': 1, 'vhdp': 1, 'vhd': 2, 'vhcx': 1, 'vhcc': 1, 'vhb': 7, 'vhad': 1, 'vha': 3, 'vh': 9, 'vgpf': 1, 'vgmh': 1, 'vghq': 1, 'vghm': 1, 'vgh': 3, 'vgc': 1, 'vga': 1, 'vg': 10, 'vfyz': 1, 'vfw': 1, 'vfqk': 1, 'vfnb': 1, 'vfmb': 1, 'vfk': 1, 'vfjx': 1, 'vfjc': 1, 'vfjb': 1, 'vfh': 2, 'vfgh': 1, 'vfdy': 1, 'vfc': 2, 'vfbj': 1, 'vfa': 1, 'vf': 1, 'vdzw': 1, 'vdyc': 1, 'vdp': 1, 'vdb': 1, 'vd': 6, 'vcx': 1, 'vcwg': 1, 'vcqf': 1, 'vcqb': 1, 'vcpf': 1, 'vcp': 2, 'vcnj': 1, 'vcmy': 1, 'vcm': 2, 'vckd': 1, 'vcjb': 1, 'vch': 3, 'vcg': 1, 'vcft': 1, 'vcfc': 1, 'vcc': 1, 'vcb': 2, 'vcat': 2, 'vc': 13, 'vbz': 1, 'vbyk': 1, 'vbya': 1, 'vbwp': 1, 'vbwj': 1, 'vbwb': 1, 'vbw': 4, 'vbt': 1, 'vbny': 1, 'vbn': 1, 'vbky': 1, 'vbh': 2, 'vbf': 1, 'vbdi': 1, 'vbci': 1, 'vbc': 1, 'vbbx': 1, 'vbbc': 1, 'vbba': 1, 'vba': 1, 'vb': 15, 'vawz': 1, 'vaww': 1, 'vap': 1, 'vam': 1, 'vakh': 1, 'vakc': 1, 'vak': 1, 'vag': 1, 'vach': 1, 'vabk': 1, 'va': 4, 'tzwg': 1, 'tzw': 1, 'tzqx': 1, 'tzhw': 1, 'tzhv': 1, 'tzg': 2, 'tzc': 2, 'tzba': 1, 'tzb': 1, 'tza': 1, 'tz': 7, 'tyzn': 1, 'tyxv': 1, 'tywm': 1, 'tywk': 1, 'tyw': 3, 'tyv': 1, 'tyqa': 1, 'tyq': 1, 'typ': 1, 'tyn': 1, 'tymh': 1, 'tykn': 1, 'tyk': 1, 'tyiw': 1, 'tyhw': 1, 'tyh': 2, 'tyf': 1, 'tyd': 1, 'tyc': 1, 'tybh': 2, 'tyb': 1, 'ty': 20, 'txzq': 1, 'txz': 1, 'txyb': 1, 'txy': 1, 'txwg': 1, 'txw': 1, 'txvz': 1, 'txq': 1, 'txp': 1, 'txm': 1, 'txib': 1, 'txhi': 1, 'txh': 1, 'txfq': 1, 'txcw': 1, 'txc': 1, 'txb': 1, 'txay': 1, 'txa': 1, 'tx': 6, 'twyc': 1, 'twy': 1, 'twxy': 1, 'twxg': 1, 'twwi': 1, 'twqn': 1, 'twq': 2, 'twph': 1, 'twp': 1, 'twkj': 1, 'twk': 3, 'twjk': 1, 'twj': 1, 'twhk': 1, 'twh': 1, 'twgk': 1, 'twgh': 1, 'twgc': 1, 'twg': 2, 'twcy': 1, 'twck': 1, 'twc': 1, 'twbk': 1, 'twbj': 1, 'twbb': 1, 'twb': 2, 'tw': 7, 'tvz': 1, 'tvx': 2, 'tvwc': 1, 'tvw': 2, 'tvi': 1, 'tvh': 1, 'tvg': 1, 'tvcx': 1, 'tvbb': 1, 'tv': 4, 'tqyp': 1, 'tqmb': 1, 'tqhx': 1, 'tqbc': 1, 'tqb': 1, 'tqaz': 1, 'tqai': 1, 'tqac': 1, 'tqab': 1, 'tq': 8, 'tpy': 1, 'tpx': 1, 'tpv': 1, 'tpnh': 1, 'tpm': 1, 'tph': 1, 'tpfk': 1, 'tpcy': 1, 'tpb': 3, 'tp': 5, 'tnzd': 1, 'tnz': 2, 'tnyb': 1, 'tnx': 1, 'tnwj': 1, 'tnq': 1, 'tnph': 1, 'tnkz': 1, 'tnhd': 1, 'tncw': 1, 'tnbm': 1, 'tnb': 2, 'tn': 14, 'tmw': 1, 'tmv': 1, 'tmja': 1, 'tmj': 2, 'tmhc': 1, 'tmf': 1, 'tmc': 2, 'tmbz': 1, 'tmbb': 1, 'tmb': 1, 'tm': 5, 'tkyb': 1, 'tky': 1, 'tkpc': 1, 'tkp': 1, 'tkjc': 1, 'tkj': 1, 'tkig': 1, 'tkh': 2, 'tkbc': 1, 'tk': 3, 'tjv': 1, 'tji': 1, 'tjh': 2, 'tjgv': 1, 'tjbm': 1, 'tjb': 2, 'tj': 6, 'tiyf': 1, 'tiy': 2, 'tiwq': 1, 'tiqk': 1, 'tip': 1, 'tim': 1, 'tik': 1, 'tih': 1, 'tifz': 2, 'tid': 1, 'tic': 1, 'tiay': 1, 'ti': 2, 'thy': 2, 'thwx': 1, 'thwd': 1, 'thvy': 1, 'thv': 1, 'thpk': 1, 'thp': 1, 'thnx': 1, 'thn': 2, 'thmv': 1, 'thm': 1, 'thk': 2, 'thi': 1, 'thgv': 1, 'thfb': 1, 'thdz': 1, 'thc': 1, 'thby': 1, 'thb': 1, 'thay': 1, 'th': 11, 'tgw': 1, 'tgvh': 1, 'tgqy': 1, 'tgqn': 1, 'tgq': 1, 'tgn': 1, 'tgij': 1, 'tghw': 1, 'tghb': 1, 'tgf': 1, 'tgdb': 1, 'tgcz': 1, 'tgc': 3, 'tgb': 1, 'tg': 7, 'tfz': 1, 'tfwx': 1, 'tfwj': 1, 'tfqd': 1, 'tfq': 1, 'tfhn': 2, 'tfh': 1, 'tfcj': 1, 'tfc': 1, 'tf': 3, 'tdym': 1, 'tdy': 1, 'tdwz': 1, 'tdw': 1, 'tdp': 1, 'tdnb': 1, 'tdn': 1, 'tdj': 1, 'tdh': 1, 'tdf': 1, 'tdcq': 1, 'tdc': 1, 'td': 6, 'tczw': 1, 'tcxc': 1, 'tcwi': 1, 'tcw': 2, 'tcq': 1, 'tcpw': 1, 'tcpn': 1, 'tcp': 1, 'tcni': 1, 'tcmk': 1, 'tcmh': 1, 'tckx': 1, 'tcj': 1, 'tchn': 1, 'tchk': 1, 'tchc': 1, 'tcg': 1, 'tcfn': 1, 'tcf': 1, 'tcch': 1, 'tcc': 3, 'tcbk': 1, 'tcb': 2, 'tca': 2, 'tc': 19, 'tbzy': 2, 'tbzg': 1, 'tbzf': 1, 'tbyw': 1, 'tbx': 3, 'tbw': 1, 'tbv': 1, 'tbqm': 1, 'tbpz': 1, 'tbpa': 1, 'tbk': 1, 'tbjw': 1, 'tbji': 1, 'tbi': 1, 'tbhx': 1, 'tbhp': 1, 'tbhd': 1, 'tbh': 2, 'tbf': 3, 'tbdy': 1, 'tbd': 1, 'tbcq': 1, 'tbc': 2, 'tbbw': 2, 'tbb': 3, 'tbad': 1, 'tbab': 1, 'tba': 1, 'tb': 22, 'tayq': 1, 'taxb': 1, 'tawh': 1, 'taq': 2, 'tanj': 1, 'tamg': 1, 'tam': 1, 'taj': 1, 'tahy': 1, 'tagc': 1, 'taf': 1, 'taci': 1, 'tabi': 1, 'ta': 6, 'qzy': 2, 'qzw': 1, 'qzn': 1, 'qzjy': 1, 'qzi': 1, 'qzhv': 1, 'qzd': 1, 'qzc': 2, 'qzbc': 1, 'qzab': 1, 'qza': 2, 'qz': 5, 'qyxh': 1, 'qywx': 1, 'qyw': 3, 'qyvc': 1, 'qyv': 1, 'qyp': 1, 'qyn': 1, 'qyip': 1, 'qygt': 1, 'qyg': 2, 'qydw': 1, 'qydt': 1, 'qybw': 1, 'qybj': 1, 'qybd': 1, 'qybc': 1, 'qyb': 2, 'qy': 13, 'qxy': 1, 'qxv': 1, 'qxt': 1, 'qxpj': 1, 'qxm': 1, 'qxic': 1, 'qxhc': 2, 'qxh': 1, 'qxgp': 1, 'qxdp': 1, 'qxbm': 1, 'qx': 3, 'qwzw': 1, 'qwy': 3, 'qwvp': 1, 'qwv': 1, 'qwt': 2, 'qwmg': 1, 'qwjy': 1, 'qwiw': 1, 'qwid': 1, 'qwic': 1, 'qwht': 1, 'qwhi': 1, 'qwhc': 1, 'qwh': 1, 'qwgh': 1, 'qwg': 1, 'qwfy': 1, 'qwcy': 1, 'qwcj': 1, 'qwc': 2, 'qwb': 3, 'qw': 9, 'qvyz': 1, 'qvyp': 1, 'qvy': 1, 'qvxn': 1, 'qvw': 1, 'qvht': 1, 'qvh': 1, 'qvfx': 1, 'qvcb': 1, 'qvc': 1, 'qv': 7, 'qtzw': 1, 'qtzc': 1, 'qtyi': 1, 'qtv': 1, 'qtj': 2, 'qti': 1, 'qthg': 1, 'qtgc': 1, 'qtfp': 1, 'qtf': 2, 'qtdx': 1, 'qt': 6, 'qpz': 1, 'qpyw': 1, 'qpxv': 1, 'qpx': 1, 'qpw': 1, 'qpty': 1, 'qpmf': 1, 'qpg': 2, 'qpdh': 1, 'qpbw': 1, 'qpbv': 1, 'qpb': 2, 'qpac': 1, 'qp': 8, 'qny': 1, 'qnwf': 1, 'qnw': 2, 'qnk': 1, 'qnhy': 1, 'qnhd': 1, 'qngt': 1, 'qngb': 2, 'qndt': 1, 'qndk': 1, 'qnc': 1, 'qn': 3, 'qmzy': 1, 'qmz': 1, 'qmxw': 1, 'qmtw': 1, 'qmt': 1, 'qmkh': 1, 'qmi': 1, 'qmh': 2, 'qmgz': 1, 'qmcw': 1, 'qmcb': 1, 'qmc': 1, 'qmb': 1, 'qmaj': 1, 'qmah': 1, 'qm': 9, 'qkxb': 1, 'qkww': 1, 'qkp': 1, 'qkma': 1, 'qkg': 1, 'qkd': 2, 'qkcy': 1, 'qkcv': 1, 'qkc': 1, 'qkbb': 1, 'qkb': 2, 'qkab': 1, 'qk': 6, 'qjyi': 1, 'qjy': 1, 'qjwy': 1, 'qjwt': 1, 'qjv': 1, 'qjnf': 1, 'qjhw': 1, 'qjb': 1, 'qj': 8, 'qipj': 1, 'qin': 1, 'qij': 1, 'qihw': 1, 'qihh': 1, 'qih': 1, 'qig': 1, 'qid': 1, 'qibw': 1, 'qibj': 1, 'qia': 1, 'qi': 7, 'qhzd': 1, 'qhzc': 1, 'qhzb': 1, 'qhz': 1, 'qhym': 1, 'qhy': 2, 'qhwn': 1, 'qhw': 2, 'qhv': 1, 'qhtw': 1, 'qhp': 1, 'qhn': 1, 'qhkc': 1, 'qhk': 1, 'qhjt': 1, 'qhjb': 1, 'qhgw': 1, 'qhgk': 1, 'qhg': 1, 'qhf': 1, 'qhd': 1, 'qhcv': 1, 'qhbz': 1, 'qhbd': 1, 'qhb': 4, 'qh': 11, 'qgz': 1, 'qgyh': 1, 'qgy': 1, 'qgvd': 1, 'qgfb': 1, 'qgcd': 1, 'qgc': 1, 'qg': 6, 'qfxm': 1, 'qfwt': 1, 'qftb': 1, 'qft': 1, 'qfdp': 1, 'qfdb': 1, 'qfcm': 1, 'qfcb': 1, 'qfbi': 1, 'qfb': 1, 'qfa': 1, 'qf': 5, 'qdzb': 1, 'qdyk': 1, 'qdyc': 1, 'qdya': 1, 'qdy': 1, 'qdm': 1, 'qdi': 1, 'qdhg': 1, 'qdhc': 1, 'qdba': 1, 'qd': 9, 'qczn': 1, 'qcz': 1, 'qcym': 1, 'qcyk': 1, 'qcx': 1, 'qcww': 1, 'qcwf': 1, 'qcwd': 1, 'qcw': 1, 'qcvz': 1, 'qcpz': 1, 'qcn': 2, 'qcm': 1, 'qck': 2, 'qcjw': 1, 'qci': 2, 'qchy': 1, 'qch': 1, 'qcc': 1, 'qcbi': 1, 'qcb': 1, 'qca': 1, 'qc': 14, 'qbzh': 1, 'qbzb': 1, 'qbz': 1, 'qbyg': 1, 'qbyd': 1, 'qbyb': 1, 'qby': 1, 'qbxw': 1, 'qbxj': 1, 'qbx': 1, 'qbna': 1, 'qbn': 3, 'qbm': 1, 'qbkg': 1, 'qbj': 1, 'qbix': 1, 'qbhf': 1, 'qbh': 1, 'qbgy': 1, 'qbgj': 1, 'qbgi': 1, 'qbg': 1, 'qbd': 3, 'qbcp': 1, 'qbcn': 1, 'qbcj': 1, 'qbch': 1, 'qbc': 3, 'qbbm': 1, 'qbbh': 1, 'qbb': 1, 'qbah': 1, 'qba': 1, 'qb': 23, 'qaw': 1, 'qaph': 1, 'qand': 1, 'qakd': 1, 'qaiy': 1, 'qah': 1, 'qad': 1, 'qac': 1, 'qaby': 1, 'qab': 1, 'qa': 6, 'pzxy': 1, 'pzx': 1, 'pzty': 1, 'pzkq': 1, 'pzcy': 1, 'pzcd': 1, 'pzbx': 1, 'pzbm': 1, 'pzbb': 1, 'pz': 9, 'pyyn': 1, 'pyy': 1, 'pyx': 1, 'pywy': 1, 'pywt': 1, 'pywc': 1, 'pyw': 1, 'pytw': 1, 'pyt': 1, 'pyqb': 1, 'pymk': 1, 'pymh': 1, 'pykg': 1, 'pykf': 1, 'pykb': 1, 'pyk': 1, 'pyj': 1, 'pyhv': 1, 'pyhj': 1, 'pyh': 1, 'pyf': 1, 'pycb': 1, 'pyc': 1, 'pyb': 1, 'pya': 2, 'py': 15, 'pxyk': 1, 'pxy': 1, 'pxm': 1, 'pxjc': 1, 'pxj': 1, 'pxi': 2, 'pxhi': 1, 'pxhh': 1, 'pxhd': 2, 'pxdk': 1, 'pxc': 1, 'pxb': 2, 'px': 11, 'pwyb': 1, 'pwy': 1, 'pwx': 1, 'pwwt': 1, 'pwwq': 1, 'pww': 1, 'pwv': 1, 'pwt': 1, 'pwqk': 1, 'pwmc': 2, 'pwj': 1, 'pwi': 1, 'pwhi': 1, 'pwh': 2, 'pwg': 1, 'pwc': 2, 'pwb': 4, 'pwac': 1, 'pw': 10, 'pvx': 1, 'pvww': 1, 'pvwi': 1, 'pvq': 1, 'pvhc': 1, 'pvh': 1, 'pvg': 2, 'pvfy': 1, 'pvfb': 1, 'pvf': 1, 'pvcm': 1, 'pvc': 1, 'pvb': 2, 'pva': 1, 'pv': 7, 'ptx': 1, 'ptwh': 1, 'ptwa': 1, 'ptw': 1, 'ptv': 1, 'ptqa': 1, 'ptq': 1, 'ptkq': 1, 'ptkh': 1, 'ptj': 1, 'pti': 1, 'ptdw': 1, 'ptb': 1, 'ptag': 1, 'pta': 1, 'pt': 2, 'pqzv': 1, 'pqyd': 1, 'pqy': 2, 'pqv': 2, 'pqmv': 1, 'pqmf': 1, 'pqit': 1, 'pqdz': 1, 'pqd': 1, 'pqbw': 1, 'pqbf': 1, 'pq': 7, 'pnya': 1, 'pnwf': 1, 'pnwb': 1, 'pnt': 1, 'pnmy': 1, 'pnm': 1, 'pnhb': 1, 'png': 1, 'pnct': 1, 'pnc': 1, 'pnbz': 1, 'pnb': 1, 'pn': 7, 'pmy': 1, 'pmq': 1, 'pmn': 1, 'pmic': 1, 'pmi': 1, 'pmhb': 1, 'pmh': 1, 'pmfh': 1, 'pmcn': 1, 'pmcb': 1, 'pmbh': 1, 'pmb': 1, 'pma': 1, 'pm': 9, 'pkz': 1, 'pky': 2, 'pkxg': 1, 'pkx': 1, 'pkwq': 1, 'pkwm': 1, 'pkw': 1, 'pkqg': 1, 'pkjh': 1, 'pkjc': 1, 'pkhw': 1, 'pkgw': 1, 'pkbw': 1, 'pkb': 2, 'pk': 5, 'pjzi': 1, 'pjy': 1, 'pjwa': 1, 'pjvy': 1, 'pjt': 1, 'pjq': 1, 'pjn': 1, 'pjkb': 1, 'pji': 1, 'pjfx': 1, 'pjf': 1, 'pjbw': 1, 'pj': 4, 'piyh': 1, 'pix': 2, 'piwh': 1, 'piwf': 1, 'piw': 3, 'pimh': 1, 'pihy': 1, 'pihb': 1, 'pig': 1, 'picy': 1, 'pic': 2, 'pibc': 2, 'pib': 1, 'pi': 4, 'phww': 1, 'phw': 3, 'phv': 1, 'phn': 1, 'phk': 1, 'phjy': 1, 'phjk': 1, 'phh': 3, 'phfz': 1, 'phfh': 1, 'phfc': 1, 'phdt': 1, 'phci': 1, 'phcc': 1, 'phc': 2, 'phby': 1, 'phbn': 1, 'phbm': 1, 'phb': 1, 'pha': 2, 'ph': 11, 'pgxw': 1, 'pgw': 1, 'pgnf': 1, 'pgj': 1, 'pghc': 1, 'pgcy': 1, 'pgcw': 1, 'pgbm': 1, 'pgb': 2, 'pg': 3, 'pfxh': 1, 'pfx': 2, 'pfwy': 1, 'pfv': 1, 'pfq': 1, 'pfkg': 1, 'pfj': 1, 'pfhd': 1, 'pfcm': 1, 'pfcb': 1, 'pfc': 1, 'pfbg': 1, 'pfbd': 1, 'pf': 6, 'pdz': 1, 'pdyt': 1, 'pdy': 1, 'pdwc': 1, 'pdm': 2, 'pdk': 1, 'pdh': 1, 'pdf': 1, 'pdch': 1, 'pdcb': 1, 'pdbm': 1, 'pdbc': 1, 'pd': 3, 'pcz': 2, 'pcyy': 1, 'pcyi': 1, 'pcy': 1, 'pcxt': 1, 'pcwg': 1, 'pcw': 1, 'pcv': 1, 'pcqy': 1, 'pcq': 1, 'pcmh': 1, 'pcmb': 1, 'pcm': 1, 'pchz': 1, 'pchi': 1, 'pchh': 1, 'pch': 1, 'pcgw': 1, 'pcf': 2, 'pcdm': 1, 'pcd': 1, 'pccq': 1, 'pcch': 1, 'pccf': 1, 'pcbh': 1, 'pcba': 1, 'pcb': 3, 'pcaq': 1, 'pc': 15, 'pbyi': 1, 'pbyc': 1, 'pbya': 1, 'pby': 1, 'pbxh': 1, 'pbxf': 1, 'pbx': 1, 'pbwn': 1, 'pbw': 1, 'pbvn': 1, 'pbvi': 1, 'pbv': 1, 'pbn': 3, 'pbkn': 1, 'pbkc': 1, 'pbk': 1, 'pbj': 1, 'pbih': 1, 'pbhz': 1, 'pbhf': 1, 'pbhb': 1, 'pbh': 2, 'pbdh': 1, 'pbd': 2, 'pbcx': 1, 'pbcf': 1, 'pbc': 1, 'pbbv': 1, 'pbbq': 1, 'pbbc': 1, 'pbb': 2, 'pbac': 1, 'pba': 1, 'pb': 16, 'paz': 1, 'payc': 1, 'pay': 1, 'pawy': 1, 'pavc': 1, 'patn': 1, 'panb': 1, 'pahf': 1, 'pa': 6, 'nzwv': 1, 'nzw': 3, 'nzkd': 1, 'nzgy': 1, 'nzfw': 1, 'nzfa': 1, 'nzf': 1, 'nzcf': 1, 'nzc': 1, 'nzbk': 1, 'nzb': 2, 'nz': 2, 'nyyz': 1, 'nyya': 1, 'nyx': 2, 'nywi': 1, 'nywh': 1, 'nyw': 1, 'nyvw': 1, 'nyq': 3, 'nykf': 1, 'nykb': 1, 'nyja': 1, 'nyh': 1, 'nyg': 1, 'nyfw': 1, 'nyf': 2, 'nyd': 1, 'nyb': 2, 'nyaz': 1, 'nya': 2, 'ny': 14, 'nxwc': 1, 'nxtp': 1, 'nxpb': 1, 'nxh': 1, 'nxfi': 1, 'nxfc': 1, 'nxc': 1, 'nxb': 1, 'nx': 8, 'nwzf': 1, 'nwyc': 1, 'nwyb': 1, 'nwxv': 1, 'nwx': 1, 'nwvp': 1, 'nwv': 1, 'nwt': 1, 'nwqh': 1, 'nwq': 1, 'nwp': 2, 'nwkh': 1, 'nwjz': 1, 'nwja': 1, 'nwhg': 1, 'nwgt': 1, 'nwgh': 1, 'nwg': 1, 'nwc': 1, 'nwb': 4, 'nwav': 1, 'nwaf': 1, 'nw': 9, 'nvxw': 1, 'nvx': 1, 'nvty': 1, 'nvt': 3, 'nvm': 1, 'nvkg': 1, 'nvh': 1, 'nvf': 1, 'nvb': 1, 'nv': 12, 'ntzx': 1, 'ntzj': 1, 'ntz': 1, 'ntvd': 1, 'ntib': 1, 'ntfb': 1, 'ntb': 1, 'ntap': 1, 'nt': 8, 'nqzk': 1, 'nqy': 2, 'nqwb': 1, 'nqw': 1, 'nqvb': 1, 'nqtc': 1, 'nqt': 1, 'nqmd': 1, 'nqi': 1, 'nqgy': 1, 'nqcb': 1, 'nqbc': 2, 'nqap': 1, 'nqa': 2, 'nq': 11, 'npyh': 1, 'npx': 1, 'npv': 1, 'npmk': 1, 'npk': 1, 'npg': 1, 'npf': 1, 'npdb': 1, 'npbb': 1, 'npb': 2, 'np': 2, 'nmz': 1, 'nmy': 1, 'nmxt': 1, 'nmxp': 1, 'nmvk': 1, 'nmqv': 1, 'nmpb': 1, 'nmjy': 1, 'nmh': 2, 'nmfp': 1, 'nmc': 1, 'nmbb': 1, 'nmb': 1, 'nm': 4, 'nkyq': 1, 'nkyb': 1, 'nkxh': 1, 'nkx': 1, 'nkwy': 1, 'nkj': 1, 'nkhv': 1, 'nkgv': 1, 'nkc': 1, 'nkb': 1, 'nk': 7, 'njzt': 1, 'njx': 1, 'njw': 1, 'njvd': 1, 'njv': 1, 'njtq': 1, 'njpb': 1, 'njp': 1, 'njha': 1, 'njfy': 1, 'njfc': 1, 'njd': 1, 'njc': 1, 'njbc': 1, 'njb': 1, 'nj': 8, 'niyp': 1, 'nix': 1, 'niw': 2, 'nivb': 1, 'nip': 2, 'nijc': 1, 'nic': 1, 'niba': 1, 'ni': 4, 'nhyw': 1, 'nhy': 3, 'nhwf': 1, 'nhwb': 1, 'nhw': 1, 'nhvh': 1, 'nhv': 1, 'nhty': 1, 'nhqk': 1, 'nhqb': 1, 'nhp': 2, 'nhmk': 1, 'nhk': 1, 'nhj': 1, 'nhhy': 1, 'nhhq': 1, 'nhhb': 1, 'nhh': 1, 'nhgj': 1, 'nhgc': 1, 'nhd': 1, 'nhcq': 1, 'nhc': 1, 'nhbi': 1, 'nhb': 5, 'nh': 12, 'ngx': 1, 'ngwq': 1, 'ngwb': 1, 'ngvz': 1, 'ngvy': 1, 'ngph': 1, 'ngj': 2, 'ngh': 1, 'ngc': 1, 'ngbk': 1, 'ngbh': 1, 'ngak': 1, 'ng': 3, 'nfy': 1, 'nfwb': 1, 'nfq': 1, 'nfph': 1, 'nfkv': 1, 'nfkb': 1, 'nfip': 1, 'nfi': 1, 'nfhb': 1, 'nfh': 1, 'nf': 8, 'ndz': 1, 'ndyb': 1, 'ndy': 1, 'ndxw': 1, 'ndwf': 1, 'ndt': 1, 'ndp': 1, 'ndkw': 1, 'ndhy': 1, 'ndh': 2, 'ndc': 1, 'ndb': 2, 'ndap': 1, 'nd': 4, 'ncza': 1, 'ncwx': 1, 'ncwh': 1, 'ncvz': 1, 'nctz': 1, 'nciv': 1, 'ncht': 1, 'nch': 3, 'ncgx': 1, 'ncch': 1, 'ncbh': 1, 'ncbf': 1, 'ncbc': 1, 'ncb': 3, 'nca': 1, 'nc': 9, 'nbzx': 1, 'nbzh': 1, 'nbz': 1, 'nbyd': 1, 'nbyb': 1, 'nby': 1, 'nbx': 2, 'nbwh': 1, 'nbwa': 1, 'nbtd': 1, 'nbt': 1, 'nbq': 2, 'nbp': 3, 'nbmi': 1, 'nbma': 1, 'nbk': 2, 'nbjw': 1, 'nbji': 1, 'nbjh': 1, 'nbiy': 1, 'nbht': 1, 'nbhp': 1, 'nbh': 1, 'nbgk': 1, 'nbft': 1, 'nbfd': 1, 'nbdk': 1, 'nbdc': 1, 'nbcj': 1, 'nbch': 1, 'nbcb': 1, 'nbc': 2, 'nbbx': 1, 'nbb': 3, 'nb': 15, 'nay': 1, 'naxq': 1, 'nax': 1, 'natk': 1, 'naq': 1, 'nap': 1, 'nag': 2, 'nafq': 1, 'na': 5, 'mzy': 1, 'mzn': 1, 'mzk': 1, 'mzib': 1, 'mzhw': 1, 'mzh': 1, 'mzbx': 1, 'mzbd': 1, 'mzb': 1, 'mz': 2, 'myz': 1, 'myyg': 1, 'myy': 1, 'myxc': 1, 'mywi': 1, 'myw': 1, 'myvh': 1, 'mytc': 1, 'mytb': 1, 'myq': 1, 'mykz': 1, 'mykh': 1, 'myk': 1, 'myjc': 1, 'myiv': 1, 'myg': 1, 'myc': 1, 'mybt': 1, 'mybb': 1, 'myb': 2, 'my': 14, 'mxyz': 1, 'mxyc': 1, 'mxyb': 1, 'mxy': 2, 'mxwv': 1, 'mxwj': 1, 'mxw': 1, 'mxv': 1, 'mxq': 1, 'mxnh': 1, 'mxj': 1, 'mxi': 1, 'mxhk': 1, 'mxhi': 1, 'mxcz': 1, 'mxcg': 1, 'mxcc': 1, 'mxc': 1, 'mxb': 1, 'mxay': 1, 'mxa': 1, 'mx': 7, 'mwz': 1, 'mwyj': 1, 'mwy': 1, 'mwxh': 1, 'mwxb': 1, 'mwx': 2, 'mww': 2, 'mwtd': 1, 'mwt': 1, 'mwpv': 1, 'mwpn': 1, 'mwpk': 1, 'mwpb': 1, 'mwp': 1, 'mwn': 2, 'mwk': 3, 'mwjy': 1, 'mwj': 2, 'mwgb': 1, 'mwfc': 1, 'mwf': 1, 'mwcz': 1, 'mwcq': 1, 'mwch': 1, 'mwcd': 1, 'mwc': 3, 'mwb': 2, 'mwaf': 1, 'mwab': 1, 'mw': 15, 'mvz': 2, 'mvy': 1, 'mvtf': 1, 'mvta': 1, 'mvqi': 1, 'mvjc': 1, 'mvg': 1, 'mvfd': 1, 'mvdn': 1, 'mvd': 2, 'mvc': 1, 'mvb': 1, 'mv': 6, 'mtz': 2, 'mtw': 1, 'mtqc': 1, 'mtp': 1, 'mtk': 1, 'mtih': 1, 'mth': 1, 'mtd': 1, 'mtav': 1, 'mt': 14, 'mqwv': 1, 'mqn': 1, 'mqj': 1, 'mqfc': 1, 'mqdz': 1, 'mqcy': 1, 'mqch': 1, 'mqc': 1, 'mqba': 1, 'mq': 5, 'mpy': 1, 'mpx': 1, 'mpw': 1, 'mpjt': 1, 'mpjq': 1, 'mpiz': 1, 'mph': 1, 'mpg': 1, 'mpfy': 1, 'mpdc': 1, 'mpc': 2, 'mpb': 3, 'mp': 3, 'mnzw': 1, 'mnyp': 1, 'mnxd': 1, 'mnx': 1, 'mnw': 3, 'mnpa': 1, 'mnp': 1, 'mnkh': 1, 'mnid': 1, 'mn': 8, 'mkyg': 1, 'mkiq': 1, 'mkh': 1, 'mkf': 1, 'mkbj': 1, 'mkb': 1, 'mka': 1, 'mk': 4, 'mjy': 1, 'mjw': 1, 'mjq': 2, 'mjk': 1, 'mjcw': 1, 'mjbf': 1, 'mj': 5, 'miyz': 1, 'miyx': 1, 'miw': 2, 'mip': 2, 'mihw': 1, 'mihf': 1, 'micc': 1, 'mic': 1, 'mi': 9, 'mhz': 2, 'mhyw': 1, 'mhyh': 1, 'mhx': 1, 'mhwn': 1, 'mhw': 1, 'mhv': 1, 'mht': 1, 'mhp': 2, 'mhnj': 1, 'mhfw': 1, 'mhf': 2, 'mhdc': 1, 'mhch': 1, 'mhcg': 1, 'mhcf': 1, 'mhc': 1, 'mhb': 2, 'mhay': 1, 'mhad': 1, 'mh': 11, 'mgyy': 1, 'mgxk': 1, 'mgwv': 1, 'mgv': 1, 'mgt': 1, 'mgqc': 1, 'mgk': 2, 'mgf': 1, 'mgcw': 1, 'mgby': 1, 'mgb': 2, 'mg': 5, 'mfzy': 1, 'mfzb': 1, 'mfy': 1, 'mfxq': 1, 'mfx': 1, 'mfwz': 2, 'mfq': 1, 'mfkz': 1, 'mfhz': 1, 'mfhh': 1, 'mfgq': 2, 'mfg': 1, 'mfdh': 1, 'mfd': 1, 'mfcd': 1, 'mfc': 1, 'mfb': 1, 'mf': 6, 'mdyf': 1, 'mdy': 1, 'mdxp': 1, 'mdx': 1, 'mdw': 2, 'mdtj': 1, 'mdnk': 1, 'mdn': 1, 'mdhy': 1, 'mdhn': 1, 'mdh': 1, 'mdgi': 1, 'mdcz': 1, 'mdbw': 1, 'md': 7, 'mczy': 1, 'mczc': 1, 'mcy': 2, 'mcxb': 1, 'mcwi': 1, 'mcwh': 1, 'mcw': 1, 'mcty': 1, 'mctk': 1, 'mct': 1, 'mcqy': 1, 'mcqx': 1, 'mcn': 1, 'mckz': 1, 'mck': 1, 'mcjy': 1, 'mcj': 1, 'mci': 1, 'mchc': 1, 'mcfb': 1, 'mccx': 1, 'mccn': 1, 'mcbq': 1, 'mcb': 1, 'mc': 11, 'mbyp': 1, 'mbya': 1, 'mby': 4, 'mbwq': 1, 'mbvn': 1, 'mbv': 1, 'mbty': 1, 'mbtw': 1, 'mbtc': 1, 'mbq': 2, 'mbp': 1, 'mbnp': 1, 'mbng': 1, 'mbn': 1, 'mbkh': 1, 'mbjf': 1, 'mbjb': 1, 'mbiz': 1, 'mbiw': 1, 'mbic': 1, 'mbi': 1, 'mbha': 1, 'mbh': 2, 'mbfi': 1, 'mbf': 1, 'mbdi': 1, 'mbdh': 1, 'mbd': 1, 'mbcw': 1, 'mbca': 1, 'mbc': 2, 'mbbk': 1, 'mbbi': 1, 'mbbg': 1, 'mbb': 6, 'mbaf': 1, 'mb': 15, 'mazc': 1, 'maxh': 1, 'mawh': 1, 'maw': 1, 'mav': 1, 'matz': 1, 'manj': 1, 'mak': 1, 'mac': 1, 'mabw': 1, 'mab': 1, 'ma': 7, 'kzym': 1, 'kzx': 1, 'kzqc': 1, 'kzp': 1, 'kzha': 1, 'kzh': 2, 'kzgw': 1, 'kzgh': 1, 'kzd': 2, 'kzcp': 1, 'kzch': 1, 'kzc': 1, 'kzbc': 1, 'kzb': 1, 'kz': 6, 'kyyc': 1, 'kyy': 1, 'kyxc': 1, 'kyw': 2, 'kyvz': 1, 'kytc': 1, 'kyqa': 1, 'kypx': 1, 'kypn': 1, 'kyph': 1, 'kynb': 1, 'kym': 1, 'kyib': 1, 'kyh': 1, 'kygn': 1, 'kyg': 1, 'kyfq': 1, 'kyd': 2, 'kycw': 1, 'kyci': 1, 'kyc': 1, 'kyb': 3, 'ky': 10, 'kxz': 2, 'kxw': 1, 'kxt': 1, 'kxf': 1, 'kx': 9, 'kwz': 1, 'kww': 1, 'kwv': 1, 'kwtp': 1, 'kwnw': 1, 'kwn': 1, 'kwm': 1, 'kwg': 1, 'kwd': 1, 'kwcj': 1, 'kwcb': 1, 'kwca': 1, 'kwc': 2, 'kwbw': 1, 'kwb': 2, 'kwa': 1, 'kw': 15, 'kvz': 1, 'kvxw': 1, 'kvw': 1, 'kvhz': 1, 'kvhd': 1, 'kvh': 1, 'kvd': 1, 'kvc': 1, 'kvbd': 1, 'kva': 1, 'kv': 5, 'ktz': 1, 'ktvw': 1, 'ktv': 1, 'ktpv': 1, 'ktn': 1, 'ktfb': 1, 'ktdz': 1, 'ktd': 1, 'ktcg': 1, 'ktay': 1, 'kt': 3, 'kqww': 1, 'kqwt': 1, 'kqwn': 1, 'kqt': 1, 'kqnd': 1, 'kqmg': 1, 'kqm': 1, 'kqj': 1, 'kqd': 2, 'kq': 4, 'kpz': 1, 'kpqn': 1, 'kpq': 1, 'kpdt': 1, 'kpai': 1, 'kp': 5, 'knw': 1, 'knt': 1, 'knp': 1, 'knhx': 1, 'knf': 1, 'knc': 2, 'knb': 2, 'kn': 12, 'kmyz': 1, 'kmy': 1, 'kmwj': 1, 'kmh': 1, 'kmgv': 1, 'kmdc': 1, 'kmcz': 1, 'kmc': 1, 'kmby': 1, 'kmbf': 1, 'kmbd': 1, 'kmbc': 1, 'km': 7, 'kjz': 1, 'kjxf': 1, 'kjx': 1, 'kjw': 1, 'kjta': 1, 'kjt': 1, 'kjhm': 1, 'kjgy': 1, 'kjf': 1, 'kjcw': 1, 'kjcn': 1, 'kjb': 2, 'kja': 1, 'kj': 9, 'kiyw': 1, 'kiy': 1, 'kixg': 1, 'kiwj': 1, 'kiw': 1, 'kivy': 1, 'kivd': 1, 'kiv': 1, 'kiqy': 2, 'kip': 1, 'kiny': 1, 'kih': 1, 'kif': 1, 'kid': 1, 'kicc': 1, 'kiaj': 1, 'ki': 10, 'khz': 1, 'khyb': 1, 'khw': 2, 'kht': 1, 'khqp': 1, 'khq': 1, 'khph': 1, 'khp': 1, 'khn': 1, 'khmf': 1, 'khmb': 1, 'khm': 2, 'khjx': 1, 'khi': 1, 'khhv': 1, 'khdw': 1, 'khcg': 1, 'khaq': 1, 'kha': 1, 'kh': 5, 'kgyz': 1, 'kgy': 1, 'kgwz': 1, 'kgv': 1, 'kgi': 1, 'kghz': 1, 'kgdy': 1, 'kgcq': 1, 'kgbw': 1, 'kg': 6, 'kfzw': 1, 'kfyi': 1, 'kfy': 1, 'kfny': 1, 'kfmj': 1, 'kfhw': 1, 'kfhg': 1, 'kfhd': 1, 'kfgt': 1, 'kfcc': 1, 'kfc': 1, 'kfbv': 1, 'kfb': 1, 'kf': 4, 'kdzt': 1, 'kdz': 1, 'kdy': 2, 'kdw': 2, 'kdn': 1, 'kdmb': 1, 'kdj': 1, 'kdgj': 1, 'kdc': 1, 'kdbc': 1, 'kdah': 1, 'kd': 8, 'kczy': 1, 'kcz': 1, 'kcyt': 1, 'kcy': 1, 'kcxb': 1, 'kcx': 1, 'kcwx': 1, 'kcw': 1, 'kcvh': 1, 'kcvg': 1, 'kcn': 1, 'kcmy': 1, 'kcj': 1, 'kcit': 1, 'kci': 1, 'kchi': 1, 'kchb': 1, 'kcgf': 1, 'kcfa': 1, 'kcf': 1, 'kcd': 2, 'kccy': 1, 'kcby': 1, 'kcbh': 1, 'kcb': 2, 'kc': 13, 'kbzq': 1, 'kbyv': 1, 'kbyn': 1, 'kbyg': 1, 'kbyb': 1, 'kby': 1, 'kbxt': 1, 'kbxm': 1, 'kbxd': 1, 'kbx': 1, 'kbwy': 1, 'kbwb': 1, 'kbw': 3, 'kbvh': 1, 'kbvb': 1, 'kbtw': 1, 'kbt': 1, 'kbqw': 1, 'kbph': 1, 'kbn': 1, 'kbi': 1, 'kbhb': 1, 'kbh': 1, 'kbgq': 1, 'kbg': 1, 'kbfm': 1, 'kbfb': 2, 'kbd': 1, 'kbc': 4, 'kbbb': 1, 'kbah': 1, 'kbab': 1, 'kba': 1, 'kb': 17, 'kayh': 1, 'kayg': 1, 'kav': 1, 'kapm': 1, 'kan': 1, 'kajz': 1, 'kaj': 1, 'kah': 2, 'kag': 1, 'kaf': 1, 'kac': 1, 'kaby': 1, 'ka': 7, 'jzw': 3, 'jzp': 2, 'jznt': 1, 'jzm': 1, 'jzim': 1, 'jzh': 1, 'jzcx': 1, 'jzc': 2, 'jz': 5, 'jyzn': 1, 'jyzb': 1, 'jyxh': 1, 'jyx': 1, 'jywq': 1, 'jywk': 1, 'jywc': 1, 'jyvk': 1, 'jyvd': 1, 'jyt': 2, 'jyq': 2, 'jyph': 1, 'jyp': 1, 'jyn': 1, 'jymw': 1, 'jym': 1, 'jykh': 1, 'jyk': 1, 'jyht': 1, 'jyhc': 1, 'jyha': 1, 'jygi': 1, 'jygd': 1, 'jyg': 1, 'jydp': 1, 'jydh': 1, 'jyd': 2, 'jyc': 3, 'jybg': 1, 'jyb': 1, 'jy': 18, 'jxww': 1, 'jxw': 2, 'jxnz': 1, 'jxmh': 1, 'jxkc': 1, 'jxic': 1, 'jxhg': 1, 'jxh': 1, 'jxcw': 1, 'jxci': 1, 'jxbw': 1, 'jx': 6, 'jwz': 1, 'jwyx': 1, 'jwy': 1, 'jwic': 1, 'jwhx': 1, 'jwhb': 1, 'jwh': 2, 'jwf': 2, 'jwdf': 1, 'jwd': 1, 'jwcy': 1, 'jwcb': 1, 'jwc': 1, 'jwaq': 1, 'jw': 10, 'jvy': 1, 'jvwq': 1, 'jvwk': 1, 'jvqw': 1, 'jvpb': 1, 'jvmc': 1, 'jvky': 1, 'jvgd': 1, 'jvg': 2, 'jvc': 1, 'jvb': 1, 'jva': 1, 'jv': 12, 'jtyf': 1, 'jtv': 2, 'jtpx': 1, 'jtmh': 1, 'jtmg': 1, 'jtic': 1, 'jthh': 1, 'jtf': 1, 'jtd': 1, 'jtc': 1, 'jt': 7, 'jqzh': 1, 'jqz': 1, 'jqyt': 1, 'jqx': 2, 'jqvw': 1, 'jqv': 1, 'jqt': 1, 'jqp': 2, 'jqmg': 1, 'jqib': 1, 'jqhn': 1, 'jqhi': 1, 'jqcm': 1, 'jqbv': 1, 'jqah': 1, 'jq': 4, 'jpym': 1, 'jpx': 1, 'jpww': 1, 'jpwb': 1, 'jpv': 1, 'jptb': 1, 'jpqf': 1, 'jpq': 1, 'jpm': 1, 'jpi': 1, 'jphc': 1, 'jpd': 3, 'jpc': 1, 'jpbz': 1, 'jp': 7, 'jnyz': 1, 'jnxw': 1, 'jnw': 1, 'jnt': 1, 'jnq': 4, 'jnmd': 1, 'jnmb': 1, 'jnhi': 1, 'jnh': 2, 'jnf': 1, 'jncx': 1, 'jncp': 1, 'jnc': 1, 'jnb': 1, 'jn': 7, 'jmwq': 1, 'jmn': 1, 'jmi': 1, 'jmhc': 1, 'jmgb': 1, 'jmd': 1, 'jma': 1, 'jm': 6, 'jkzt': 1, 'jkyd': 1, 'jkxd': 1, 'jkwh': 1, 'jkv': 1, 'jkmp': 1, 'jkfz': 1, 'jkcb': 1, 'jkb': 1, 'jk': 4, 'jiyh': 1, 'jiwb': 1, 'jiq': 2, 'jipf': 1, 'jihq': 1, 'jibh': 1, 'jiab': 1, 'ji': 9, 'jhzp': 1, 'jhyh': 1, 'jhxz': 1, 'jhwb': 1, 'jhq': 1, 'jhpb': 1, 'jhnh': 1, 'jhn': 2, 'jhmk': 1, 'jhky': 1, 'jhk': 1, 'jhh': 2, 'jhgn': 1, 'jhgm': 1, 'jhgd': 1, 'jhg': 1, 'jhdk': 2, 'jhc': 2, 'jhb': 2, 'jh': 14, 'jgyc': 1, 'jgy': 1, 'jgtk': 1, 'jgth': 1, 'jgm': 1, 'jghm': 1, 'jgcb': 1, 'jgc': 1, 'jg': 2, 'jfzg': 1, 'jfy': 1, 'jfwm': 1, 'jfv': 1, 'jfpm': 1, 'jfp': 2, 'jfi': 1, 'jfdt': 1, 'jfcq': 1, 'jfc': 2, 'jfby': 1, 'jfb': 3, 'jf': 5, 'jdzm': 1, 'jdzh': 1, 'jdy': 1, 'jdwa': 1, 'jdvh': 1, 'jdt': 2, 'jdm': 1, 'jdi': 1, 'jdhn': 1, 'jdhc': 1, 'jdfa': 1, 'jdcv': 1, 'jdbi': 1, 'jdb': 1, 'jda': 1, 'jd': 11, 'jcyh': 1, 'jcy': 1, 'jcxb': 2, 'jcwn': 1, 'jcw': 1, 'jct': 2, 'jcpv': 1, 'jcnh': 1, 'jcm': 1, 'jck': 1, 'jchy': 1, 'jchb': 1, 'jcfb': 1, 'jcd': 2, 'jcct': 1, 'jcc': 1, 'jcbw': 1, 'jcb': 1, 'jc': 14, 'jbzh': 1, 'jbz': 1, 'jbyz': 1, 'jbyw': 1, 'jbyi': 1, 'jby': 1, 'jbx': 2, 'jbw': 3, 'jbvh': 1, 'jbtz': 1, 'jbtc': 1, 'jbt': 1, 'jbqw': 1, 'jbqk': 1, 'jbp': 1, 'jbnk': 1, 'jbnb': 2, 'jbna': 1, 'jbky': 1, 'jbk': 1, 'jbik': 1, 'jbh': 2, 'jbf': 1, 'jbdg': 1, 'jbdc': 1, 'jbd': 1, 'jbcw': 1, 'jbbz': 1, 'jbb': 4, 'jbap': 1, 'jban': 1, 'jbah': 1, 'jb': 17, 'jax': 2, 'jawh': 1, 'jawg': 1, 'jawc': 1, 'javk': 1, 'jat': 1, 'jaq': 1, 'jap': 1, 'jahh': 1, 'jahb': 1, 'jah': 1, 'jagx': 1, 'jafx': 1, 'ja': 11, 'izyt': 1, 'izy': 1, 'izx': 1, 'izwk': 1, 'izwh': 1, 'izqn': 1, 'izma': 1, 'izm': 2, 'izh': 1, 'izgj': 1, 'izfm': 1, 'izbh': 1, 'izax': 1, 'izam': 1, 'iza': 2, 'iz': 4, 'iyz': 3, 'iyy': 1, 'iyxb': 1, 'iywa': 1, 'iyw': 2, 'iyvm': 1, 'iyv': 1, 'iykn': 1, 'iyjt': 1, 'iygb': 1, 'iyg': 1, 'iyd': 1, 'iycy': 1, 'iycq': 1, 'iyc': 3, 'iybk': 1, 'iyb': 2, 'iyap': 1, 'iy': 17, 'ixw': 2, 'ixpb': 1, 'ixm': 2, 'ixj': 1, 'ixhm': 1, 'ixg': 1, 'ixfj': 1, 'ixf': 1, 'ixcq': 1, 'ix': 7, 'iwzh': 1, 'iwz': 1, 'iwyj': 1, 'iwyb': 1, 'iwy': 2, 'iwx': 1, 'iww': 1, 'iwvb': 1, 'iwkm': 1, 'iwhy': 1, 'iwg': 1, 'iwfk': 1, 'iwfb': 2, 'iwf': 1, 'iwdm': 1, 'iwcz': 1, 'iwcc': 1, 'iwc': 1, 'iwb': 1, 'iwa': 1, 'iw': 8, 'ivyt': 1, 'ivw': 1, 'ivth': 1, 'ivqt': 1, 'ivjb': 1, 'ivj': 1, 'ivcz': 1, 'ivbj': 1, 'iv': 6, 'ityx': 1, 'ity': 3, 'itx': 1, 'itwv': 1, 'itwd': 1, 'itwc': 1, 'itqb': 1, 'itkn': 1, 'itkf': 1, 'itjn': 1, 'itfc': 1, 'itcj': 1, 'itc': 1, 'itbh': 1, 'itad': 1, 'it': 3, 'iqzt': 1, 'iqy': 2, 'iqx': 1, 'iqp': 2, 'iqg': 1, 'iqcv': 1, 'iqcj': 1, 'iqc': 1, 'iqbd': 1, 'iq': 4, 'ipz': 1, 'ipwj': 1, 'ipnt': 1, 'ipmg': 1, 'ipk': 1, 'ipdh': 1, 'ipbj': 1, 'ipb': 2, 'ip': 4, 'inz': 1, 'inyf': 1, 'iny': 2, 'inw': 1, 'invh': 1, 'inqy': 1, 'inp': 1, 'ink': 2, 'inhb': 1, 'inh': 1, 'ind': 1, 'ina': 1, 'in': 11, 'imyj': 1, 'imw': 2, 'imp': 1, 'imbk': 1, 'imbc': 1, 'ima': 1, 'im': 8, 'ikxh': 1, 'ikwx': 1, 'ikwj': 1, 'ikvh': 1, 'ikv': 1, 'ikq': 1, 'ikdb': 1, 'ikcz': 1, 'ikc': 1, 'ikby': 1, 'ikbt': 1, 'ikbh': 1, 'ikbb': 1, 'ikb': 1, 'ika': 2, 'ik': 6, 'ijxc': 1, 'ijwh': 1, 'ijt': 1, 'ijqp': 1, 'ijnz': 1, 'ijmp': 1, 'ijh': 1, 'ijf': 1, 'ijcw': 1, 'ijc': 1, 'ijbx': 1, 'ijb': 2, 'ij': 5, 'ihzm': 1, 'ihzj': 1, 'ihzg': 1, 'ihzc': 1, 'ihz': 2, 'ihyw': 1, 'ihyt': 1, 'ihyp': 1, 'ihyc': 1, 'ihya': 1, 'ihxn': 1, 'ihwz': 1, 'ihwc': 1, 'ihw': 1, 'ihvd': 1, 'ihvb': 1, 'iht': 2, 'ihq': 1, 'ihnb': 1, 'ihn': 1, 'ihkf': 1, 'ihk': 3, 'ihjc': 1, 'ihj': 1, 'ihdk': 1, 'ihd': 1, 'ihcy': 1, 'ihcp': 1, 'ihbw': 1, 'ihbf': 1, 'ihb': 4, 'ih': 10, 'igyd': 1, 'igw': 2, 'igt': 1, 'igq': 1, 'igph': 1, 'ign': 2, 'igm': 1, 'ighn': 1, 'igh': 1, 'igd': 3, 'igcp': 1, 'igch': 1, 'igbj': 1, 'igb': 3, 'iga': 1, 'ig': 11, 'ifyb': 1, 'ifx': 1, 'ifv': 1, 'iftm': 1, 'ift': 2, 'ifmx': 1, 'ifm': 1, 'ifh': 3, 'ifcc': 1, 'ifbz': 1, 'ifbh': 1, 'if': 6, 'idyy': 1, 'idyn': 1, 'idyb': 1, 'idw': 1, 'idp': 1, 'idma': 1, 'idm': 1, 'idg': 1, 'idcy': 1, 'idca': 1, 'id': 8, 'icyz': 1, 'icyk': 1, 'icy': 1, 'icxn': 1, 'icxc': 1, 'icwk': 1, 'icw': 1, 'icvx': 1, 'icv': 1, 'ictj': 1, 'ict': 1, 'icn': 2, 'icmc': 1, 'icm': 2, 'icjh': 1, 'icj': 2, 'ichy': 1, 'ichv': 1, 'ichb': 1, 'ich': 1, 'icf': 3, 'icdf': 1, 'iccz': 1, 'icc': 2, 'icbb': 1, 'icb': 3, 'icax': 1, 'ica': 1, 'ic': 8, 'ibz': 3, 'ibyh': 1, 'ibxt': 1, 'ibxj': 1, 'ibx': 1, 'ibw': 1, 'ibvx': 1, 'ibpt': 1, 'ibny': 1, 'ibnj': 1, 'ibng': 1, 'ibn': 2, 'ibm': 1, 'ibkb': 1, 'ibjz': 1, 'ibjc': 1, 'ibj': 3, 'ibh': 1, 'ibf': 1, 'ibdn': 1, 'ibcw': 1, 'ibch': 1, 'ibc': 2, 'ibby': 1, 'ibbp': 1, 'ibbb': 1, 'ibb': 2, 'iba': 2, 'ib': 20, 'iat': 1, 'iaq': 1, 'iamq': 1, 'iak': 1, 'iaj': 1, 'iadb': 1, 'iach': 1, 'iabc': 1, 'iab': 2, 'ia': 4, 'hzx': 1, 'hzw': 1, 'hzvt': 1, 'hzq': 2, 'hzp': 1, 'hznb': 1, 'hzm': 1, 'hzkw': 1, 'hzkc': 1, 'hzjy': 1, 'hzjc': 1, 'hzid': 1, 'hzhj': 1, 'hzgy': 1, 'hzfh': 1, 'hzdv': 1, 'hzc': 2, 'hzbc': 1, 'hzbb': 1, 'hzb': 3, 'hz': 14, 'hyxc': 1, 'hyxb': 1, 'hywn': 1, 'hywb': 1, 'hyw': 2, 'hyvj': 1, 'hyvf': 1, 'hyv': 2, 'hyty': 1, 'hytk': 1, 'hytj': 1, 'hytc': 1, 'hyt': 2, 'hyq': 2, 'hypw': 1, 'hym': 5, 'hykg': 1, 'hyj': 1, 'hyik': 1, 'hyib': 1, 'hyhw': 1, 'hyh': 2, 'hygm': 1, 'hyg': 1, 'hyfh': 1, 'hyfb': 1, 'hyf': 1, 'hyct': 1, 'hyca': 1, 'hyc': 2, 'hyby': 1, 'hybq': 1, 'hybc': 1, 'hybb': 1, 'hyb': 1, 'hyag': 1, 'hya': 1, 'hy': 29, 'hxzm': 1, 'hxz': 1, 'hxy': 1, 'hxwi': 1, 'hxw': 1, 'hxtw': 1, 'hxtg': 1, 'hxq': 1, 'hxpy': 1, 'hxpc': 1, 'hxp': 1, 'hxnv': 1, 'hxiw': 1, 'hxhb': 1, 'hxg': 1, 'hxfy': 1, 'hxfk': 1, 'hxdf': 1, 'hxd': 1, 'hxc': 1, 'hxbk': 1, 'hxb': 4, 'hx': 7, 'hwzc': 1, 'hwyz': 1, 'hwyn': 1, 'hwy': 4, 'hwx': 1, 'hwwy': 1, 'hwwn': 1, 'hww': 3, 'hwvg': 1, 'hwvd': 1, 'hwv': 3, 'hwtb': 1, 'hwpz': 1, 'hwpm': 1, 'hwpb': 1, 'hwp': 2, 'hwnz': 1, 'hwnt': 1, 'hwnk': 1, 'hwm': 1, 'hwkx': 1, 'hwkm': 1, 'hwk': 1, 'hwjv': 1, 'hwi': 2, 'hwh': 1, 'hwga': 1, 'hwfi': 1, 'hwdw': 1, 'hwdc': 1, 'hwcw': 1, 'hwck': 1, 'hwcf': 1, 'hwcb': 1, 'hwc': 3, 'hwbb': 1, 'hwb': 2, 'hwax': 1, 'hw': 21, 'hvz': 1, 'hvyz': 1, 'hvyw': 1, 'hvy': 1, 'hvwy': 1, 'hvqj': 1, 'hvq': 1, 'hvp': 1, 'hvmy': 1, 'hvk': 1, 'hvjm': 1, 'hvjd': 1, 'hvj': 1, 'hviz': 1, 'hvhy': 1, 'hvhg': 1, 'hvd': 1, 'hvcg': 1, 'hvc': 1, 'hvb': 3, 'hvaq': 1, 'hv': 9, 'htzh': 1, 'htyy': 1, 'htxa': 1, 'htwd': 1, 'htw': 1, 'htvw': 1, 'htq': 1, 'htpc': 1, 'htp': 2, 'htmz': 1, 'htm': 2, 'htk': 2, 'hti': 1, 'htg': 1, 'htc': 1, 'htbw': 1, 'htbj': 1, 'htb': 1, 'hta': 2, 'ht': 14, 'hqzk': 1, 'hqyn': 1, 'hqy': 2, 'hqxd': 1, 'hqx': 1, 'hqwb': 1, 'hqw': 1, 'hqvp': 1, 'hqv': 1, 'hqp': 1, 'hqmf': 1, 'hqif': 1, 'hqg': 2, 'hqd': 1, 'hqck': 1, 'hqbt': 1, 'hqb': 4, 'hq': 9, 'hpzy': 1, 'hpz': 2, 'hpyz': 1, 'hpyx': 1, 'hpym': 1, 'hpy': 2, 'hpxb': 1, 'hpwh': 1, 'hpvc': 1, 'hptd': 1, 'hpqd': 1, 'hpnz': 1, 'hpm': 1, 'hpkb': 1, 'hpjn': 1, 'hpjd': 1, 'hph': 1, 'hpfx': 1, 'hpfn': 1, 'hpfc': 1, 'hpd': 1, 'hpc': 3, 'hpbz': 1, 'hpbx': 2, 'hpb': 1, 'hpab': 1, 'hp': 10, 'hnz': 1, 'hnyh': 1, 'hnya': 1, 'hny': 1, 'hnx': 1, 'hnwy': 1, 'hnwx': 1, 'hnwv': 1, 'hnwj': 1, 'hnw': 1, 'hnvc': 1, 'hnpf': 1, 'hnj': 3, 'hnic': 1, 'hnib': 1, 'hng': 2, 'hndz': 1, 'hndb': 1, 'hnd': 1, 'hnc': 1, 'hnbv': 1, 'hnbc': 1, 'hnbb': 1, 'hnaw': 1, 'hna': 1, 'hn': 9, 'hmzc': 1, 'hmzb': 1, 'hmz': 1, 'hmyj': 1, 'hmy': 1, 'hmx': 1, 'hmwk': 1, 'hmw': 1, 'hmq': 1, 'hmjh': 1, 'hmj': 3, 'hmi': 2, 'hmhf': 1, 'hmh': 1, 'hmby': 1, 'hmb': 5, 'hm': 16, 'hkz': 1, 'hky': 1, 'hkxw': 1, 'hkx': 1, 'hkw': 1, 'hktb': 1, 'hknm': 1, 'hkin': 1, 'hkim': 1, 'hkhz': 1, 'hkhn': 1, 'hkgy': 1, 'hkgc': 1, 'hkdg': 1, 'hkcq': 1, 'hkc': 1, 'hkb': 2, 'hka': 1, 'hk': 12, 'hjzn': 1, 'hjzc': 1, 'hjzb': 1, 'hjyt': 1, 'hjy': 1, 'hjxb': 1, 'hjwc': 1, 'hjw': 1, 'hjvx': 1, 'hjvq': 1, 'hjv': 2, 'hjt': 1, 'hjpw': 1, 'hjn': 1, 'hjm': 1, 'hjiy': 1, 'hjiq': 1, 'hjhm': 1, 'hjft': 1, 'hjca': 1, 'hjb': 4, 'hjax': 1, 'hj': 17, 'hizt': 1, 'hiwk': 1, 'hiwc': 1, 'hiw': 3, 'hiqg': 1, 'hip': 1, 'hin': 1, 'hikp': 1, 'hij': 2, 'hihx': 1, 'hihc': 1, 'hihb': 1, 'hih': 1, 'higw': 1, 'higf': 1, 'hig': 1, 'hidn': 1, 'hidb': 1, 'hid': 1, 'hicz': 1, 'hict': 1, 'hicj': 1, 'hiba': 1, 'hib': 3, 'hi': 15, 'hhzc': 1, 'hhy': 1, 'hhw': 1, 'hht': 1, 'hhqb': 1, 'hhpk': 1, 'hhn': 2, 'hhmj': 1, 'hhm': 1, 'hhk': 1, 'hhj': 1, 'hhiw': 1, 'hhif': 1, 'hhgc': 1, 'hhf': 1, 'hhd': 1, 'hhc': 3, 'hhbw': 1, 'hhb': 2, 'hh': 14, 'hgyj': 1, 'hgxz': 1, 'hgxv': 1, 'hgwy': 1, 'hgwp': 1, 'hgw': 1, 'hgqz': 1, 'hgqj': 1, 'hgp': 3, 'hgmx': 1, 'hgmk': 1, 'hgj': 1, 'hgim': 1, 'hgib': 1, 'hgf': 1, 'hgdy': 2, 'hgcw': 1, 'hgbw': 1, 'hgb': 4, 'hg': 11, 'hfza': 1, 'hfyz': 1, 'hfy': 2, 'hfxy': 1, 'hfxh': 1, 'hfwp': 1, 'hfw': 2, 'hfv': 1, 'hft': 1, 'hfp': 2, 'hfmx': 1, 'hfm': 1, 'hfky': 1, 'hfj': 2, 'hfhx': 1, 'hfh': 4, 'hfgv': 1, 'hfd': 1, 'hfcw': 1, 'hfc': 1, 'hfb': 2, 'hfaw': 1, 'hfav': 1, 'hfa': 1, 'hf': 10, 'hdzy': 1, 'hdzt': 1, 'hdyw': 2, 'hdyq': 1, 'hdy': 1, 'hdxp': 1, 'hdx': 1, 'hdwj': 1, 'hdw': 4, 'hdq': 1, 'hdm': 1, 'hdkc': 1, 'hdj': 1, 'hdha': 1, 'hdg': 1, 'hdf': 1, 'hdbq': 2, 'hdbb': 2, 'hd': 14, 'hcz': 1, 'hcyy': 1, 'hcyi': 1, 'hcy': 2, 'hcxv': 1, 'hcx': 1, 'hcwk': 1, 'hcwb': 1, 'hcw': 3, 'hcvz': 1, 'hcvb': 1, 'hcv': 2, 'hct': 3, 'hcqp': 1, 'hcqf': 1, 'hcq': 1, 'hcp': 1, 'hcny': 1, 'hcn': 3, 'hcmk': 1, 'hcmj': 1, 'hcma': 1, 'hcky': 1, 'hckt': 1, 'hck': 1, 'hcj': 2, 'hcit': 1, 'hchw': 1, 'hch': 2, 'hcg': 1, 'hcfz': 1, 'hcf': 1, 'hcdw': 1, 'hcdv': 1, 'hcdf': 1, 'hcd': 2, 'hcc': 2, 'hcbh': 1, 'hcbc': 1, 'hcbb': 1, 'hcb': 4, 'hca': 2, 'hc': 17, 'hbzy': 1, 'hbz': 1, 'hbyx': 1, 'hbyp': 1, 'hbyn': 1, 'hby': 3, 'hbxw': 1, 'hbxd': 1, 'hbx': 1, 'hbww': 1, 'hbwv': 1, 'hbwq': 1, 'hbvz': 1, 'hbvx': 1, 'hbv': 2, 'hbty': 1, 'hbtx': 1, 'hbtb': 1, 'hbt': 2, 'hbpf': 1, 'hbpb': 1, 'hbp': 2, 'hbny': 1, 'hbn': 1, 'hbmh': 1, 'hbm': 6, 'hbk': 4, 'hbjm': 1, 'hbjc': 1, 'hbjb': 1, 'hbi': 1, 'hbhv': 1, 'hbh': 3, 'hbgy': 1, 'hbgq': 3, 'hbg': 3, 'hbfv': 1, 'hbfp': 1, 'hbfc': 1, 'hbf': 1, 'hbcg': 1, 'hbcb': 1, 'hbc': 4, 'hbbc': 1, 'hbb': 1, 'hbav': 1, 'hbag': 1, 'hba': 1, 'hb': 30, 'hazf': 1, 'hayg': 1, 'hawq': 1, 'hawb': 1, 'havt': 1, 'hav': 1, 'hatx': 1, 'hatk': 1, 'haqn': 1, 'hapb': 1, 'hany': 1, 'hanc': 1, 'hamk': 1, 'haj': 1, 'haid': 1, 'hah': 2, 'haf': 1, 'hadv': 1, 'hacb': 1, 'hab': 3, 'ha': 8, 'gzxy': 1, 'gzx': 1, 'gzw': 1, 'gzv': 1, 'gzt': 1, 'gzqb': 1, 'gzq': 1, 'gzpc': 1, 'gzpa': 1, 'gzn': 2, 'gzm': 1, 'gzj': 1, 'gzia': 1, 'gzcb': 1, 'gzc': 1, 'gz': 5, 'gyz': 1, 'gyx': 2, 'gywm': 1, 'gytn': 1, 'gyqv': 1, 'gyqd': 1, 'gypz': 1, 'gyp': 2, 'gykb': 1, 'gyjy': 1, 'gyhp': 1, 'gyh': 2, 'gyda': 1, 'gycc': 1, 'gyc': 4, 'gybf': 1, 'gybb': 1, 'gyb': 1, 'gyam': 1, 'gyac': 1, 'gy': 11, 'gxw': 1, 'gxv': 1, 'gxpc': 1, 'gxp': 1, 'gxny': 2, 'gxit': 1, 'gxhc': 1, 'gxh': 1, 'gxd': 1, 'gxb': 1, 'gx': 12, 'gwzi': 1, 'gwz': 1, 'gwyj': 1, 'gwyb': 2, 'gwy': 2, 'gwvb': 1, 'gwpt': 1, 'gwm': 2, 'gwkv': 1, 'gwhf': 1, 'gwhc': 1, 'gwfj': 1, 'gwc': 3, 'gwbi': 1, 'gwb': 2, 'gwaq': 1, 'gwab': 1, 'gw': 18, 'gvz': 1, 'gvy': 1, 'gvt': 1, 'gvq': 1, 'gvn': 1, 'gvkw': 1, 'gvk': 2, 'gvj': 1, 'gvfp': 1, 'gvcb': 1, 'gvc': 1, 'gvbd': 1, 'gvbb': 1, 'gv': 5, 'gty': 2, 'gtv': 1, 'gtmy': 1, 'gtkx': 1, 'gtk': 1, 'gtj': 1, 'gthk': 1, 'gtb': 2, 'gt': 10, 'gqzt': 1, 'gqz': 1, 'gqy': 1, 'gqx': 1, 'gqp': 1, 'gqnz': 1, 'gqn': 1, 'gqk': 1, 'gqi': 1, 'gqhh': 1, 'gqhf': 1, 'gqhb': 1, 'gqdc': 1, 'gqd': 1, 'gqbi': 1, 'gqb': 2, 'gqa': 1, 'gq': 8, 'gpx': 1, 'gpm': 1, 'gpk': 1, 'gpij': 1, 'gph': 1, 'gpd': 1, 'gpc': 1, 'gpa': 1, 'gp': 7, 'gnvq': 1, 'gnpj': 1, 'gnkb': 1, 'gnfq': 1, 'gnd': 1, 'gncw': 1, 'gnbt': 1, 'gn': 4, 'gmyz': 1, 'gmy': 1, 'gmt': 1, 'gmqw': 1, 'gmk': 2, 'gmh': 1, 'gmbv': 1, 'gm': 9, 'gky': 3, 'gkw': 2, 'gkpw': 1, 'gkpv': 1, 'gknc': 1, 'gkn': 1, 'gkj': 1, 'gkf': 1, 'gkb': 1, 'gk': 7, 'gjz': 1, 'gjyn': 1, 'gjy': 1, 'gjxw': 1, 'gjwf': 1, 'gjtw': 1, 'gjtn': 1, 'gjq': 1, 'gjpi': 1, 'gjmf': 1, 'gjb': 1, 'gjax': 1, 'gj': 8, 'giy': 2, 'gixn': 1, 'giwy': 1, 'gipz': 1, 'gip': 1, 'gik': 1, 'gij': 1, 'gih': 1, 'gicb': 1, 'gib': 4, 'gi': 7, 'ghzp': 1, 'ghyf': 1, 'ghy': 1, 'ghvi': 1, 'ghv': 1, 'ghtn': 1, 'ghtc': 1, 'ghpy': 1, 'ghn': 1, 'ghm': 2, 'ghky': 1, 'ghh': 1, 'ghc': 1, 'ghbz': 1, 'ghby': 1, 'ghbc': 1, 'ghb': 2, 'gh': 16, 'gfz': 1, 'gfx': 1, 'gfw': 1, 'gft': 1, 'gfqj': 1, 'gfnw': 1, 'gfmk': 1, 'gfhy': 1, 'gfh': 2, 'gfc': 1, 'gfby': 1, 'gfbx': 1, 'gfbk': 1, 'gfbb': 1, 'gfa': 1, 'gf': 5, 'gdzn': 1, 'gdzm': 1, 'gdy': 2, 'gdxm': 1, 'gdx': 1, 'gdwb': 1, 'gdw': 1, 'gdvw': 1, 'gdnc': 1, 'gdkm': 1, 'gdiv': 1, 'gdi': 1, 'gdcb': 1, 'gdbb': 1, 'gd': 7, 'gczp': 1, 'gcz': 2, 'gcxt': 1, 'gcw': 2, 'gcqk': 1, 'gcmk': 1, 'gcm': 1, 'gck': 2, 'gcix': 1, 'gcfw': 1, 'gcfp': 1, 'gcf': 1, 'gcdb': 1, 'gcd': 1, 'gcbn': 1, 'gcbh': 1, 'gcbb': 1, 'gcb': 2, 'gca': 2, 'gc': 10, 'gbz': 1, 'gbyx': 1, 'gbyw': 1, 'gbyi': 1, 'gbyc': 1, 'gby': 2, 'gbwd': 1, 'gbw': 3, 'gbvf': 1, 'gbv': 1, 'gbqw': 1, 'gbqh': 1, 'gbq': 2, 'gbpy': 1, 'gbnb': 1, 'gbn': 1, 'gbmw': 1, 'gbm': 1, 'gbkc': 1, 'gbj': 2, 'gbiy': 1, 'gbiv': 1, 'gbi': 1, 'gbhz': 1, 'gbhc': 1, 'gbh': 3, 'gbfb': 1, 'gbf': 1, 'gbdv': 1, 'gbd': 2, 'gbc': 5, 'gbbq': 1, 'gbbn': 1, 'gbbi': 1, 'gbbh': 1, 'gbb': 2, 'gba': 1, 'gb': 20, 'gaz': 1, 'gawq': 1, 'gapv': 1, 'gan': 1, 'gamt': 1, 'gaj': 1, 'gahp': 1, 'gafc': 1, 'gacf': 1, 'gab': 2, 'ga': 7, 'fzy': 1, 'fzqa': 1, 'fznp': 1, 'fznb': 1, 'fzn': 1, 'fzmv': 1, 'fzmk': 1, 'fzc': 1, 'fzbn': 1, 'fzb': 2, 'fz': 5, 'fyzn': 1, 'fyz': 2, 'fyw': 3, 'fyv': 3, 'fyt': 2, 'fyqb': 1, 'fyp': 3, 'fynz': 1, 'fynv': 1, 'fyn': 1, 'fymt': 1, 'fymp': 1, 'fymn': 1, 'fym': 1, 'fykn': 1, 'fyjn': 1, 'fyht': 1, 'fydn': 1, 'fyd': 1, 'fyc': 1, 'fybv': 1, 'fybq': 1, 'fybh': 1, 'fyb': 2, 'fyad': 1, 'fy': 13, 'fxzb': 1, 'fxw': 1, 'fxqt': 2, 'fxqh': 1, 'fxny': 1, 'fxna': 1, 'fxm': 1, 'fxkh': 1, 'fxkb': 1, 'fxjd': 1, 'fxi': 1, 'fxhz': 1, 'fxgc': 1, 'fxc': 1, 'fxb': 2, 'fxat': 1, 'fx': 4, 'fww': 1, 'fwvh': 1, 'fwtc': 1, 'fwt': 1, 'fwm': 1, 'fwk': 1, 'fwj': 1, 'fwhg': 1, 'fwh': 3, 'fwda': 1, 'fwci': 1, 'fwc': 1, 'fwbm': 1, 'fwb': 1, 'fwa': 1, 'fw': 13, 'fvyw': 1, 'fvqm': 1, 'fvky': 1, 'fvk': 1, 'fvi': 1, 'fvc': 1, 'fvbw': 1, 'fvbh': 1, 'fvb': 1, 'fvac': 1, 'fva': 1, 'fv': 4, 'ftz': 1, 'fty': 2, 'ftm': 2, 'ftk': 1, 'ftgz': 1, 'ftcw': 1, 'ftba': 1, 'ft': 4, 'fqyc': 1, 'fqy': 1, 'fqwv': 1, 'fqtg': 1, 'fqp': 1, 'fqn': 1, 'fqk': 1, 'fqiz': 1, 'fqim': 1, 'fqh': 1, 'fqct': 2, 'fqch': 1, 'fqbw': 1, 'fqa': 1, 'fq': 6, 'fpyc': 2, 'fpw': 2, 'fpvm': 1, 'fpt': 2, 'fpnx': 1, 'fpnk': 1, 'fpk': 1, 'fpjx': 1, 'fpj': 1, 'fpi': 1, 'fphd': 1, 'fphc': 1, 'fpgt': 1, 'fpcd': 1, 'fpb': 1, 'fp': 5, 'fnyd': 1, 'fnyb': 1, 'fnx': 1, 'fnqj': 1, 'fnpx': 1, 'fnp': 1, 'fnkj': 1, 'fnjw': 1, 'fndb': 1, 'fnc': 2, 'fnb': 1, 'fn': 5, 'fmyq': 1, 'fmyc': 1, 'fmwk': 1, 'fmw': 2, 'fmvx': 1, 'fmqd': 1, 'fmk': 1, 'fmj': 1, 'fmhv': 1, 'fmgv': 1, 'fmg': 1, 'fmdb': 1, 'fmcd': 1, 'fmcb': 1, 'fmaw': 1, 'fm': 8, 'fkz': 1, 'fkyg': 1, 'fky': 1, 'fkw': 1, 'fkhi': 1, 'fkh': 3, 'fkg': 1, 'fkdn': 1, 'fkbj': 1, 'fkam': 1, 'fk': 10, 'fjyg': 1, 'fjy': 1, 'fjwz': 1, 'fjwb': 1, 'fjvn': 1, 'fjqc': 1, 'fjq': 2, 'fjpw': 1, 'fjpt': 1, 'fjn': 1, 'fjm': 1, 'fjkb': 1, 'fjk': 1, 'fjia': 1, 'fjgw': 1, 'fjgk': 1, 'fjc': 2, 'fjb': 1, 'fj': 11, 'fiz': 1, 'fivx': 1, 'fiv': 2, 'finj': 1, 'fijp': 1, 'fij': 1, 'fih': 1, 'fidw': 1, 'fid': 1, 'fib': 1, 'fi': 5, 'fhzp': 1, 'fhz': 1, 'fhyb': 1, 'fhy': 1, 'fhwy': 1, 'fhw': 2, 'fhvc': 1, 'fhv': 1, 'fhnx': 1, 'fhnd': 1, 'fhn': 1, 'fhmk': 1, 'fhi': 1, 'fhgb': 1, 'fhg': 1, 'fhc': 1, 'fhbd': 1, 'fh': 17, 'fgz': 1, 'fgy': 2, 'fgx': 1, 'fgvh': 1, 'fgqz': 1, 'fgkv': 1, 'fgkd': 1, 'fgjp': 1, 'fgcc': 1, 'fgab': 1, 'fga': 1, 'fg': 4, 'fdzb': 1, 'fdz': 1, 'fdyb': 1, 'fdxm': 1, 'fdxj': 1, 'fdx': 1, 'fdwh': 1, 'fdv': 1, 'fdq': 1, 'fdm': 1, 'fdk': 2, 'fdh': 1, 'fdbv': 1, 'fdbk': 1, 'fdb': 1, 'fd': 8, 'fcy': 1, 'fcwy': 1, 'fcwt': 1, 'fcw': 1, 'fct': 1, 'fcq': 1, 'fcpz': 1, 'fcn': 1, 'fcmg': 1, 'fckn': 1, 'fcjt': 1, 'fcj': 1, 'fciw': 1, 'fcim': 1, 'fcig': 1, 'fchq': 1, 'fchp': 1, 'fch': 1, 'fcg': 2, 'fcc': 2, 'fcby': 2, 'fcb': 2, 'fc': 15, 'fby': 2, 'fbx': 1, 'fbwd': 1, 'fbw': 2, 'fbtz': 1, 'fbqh': 1, 'fbq': 1, 'fbpm': 1, 'fbp': 1, 'fbnc': 1, 'fbn': 1, 'fbmd': 1, 'fbkh': 1, 'fbiw': 1, 'fbiq': 1, 'fbi': 2, 'fbhw': 1, 'fbhv': 1, 'fbhn': 1, 'fbh': 2, 'fbgq': 1, 'fbgh': 1, 'fbgc': 1, 'fbd': 1, 'fbch': 1, 'fbc': 1, 'fbbq': 1, 'fbbn': 1, 'fbb': 2, 'fbai': 1, 'fba': 1, 'fb': 23, 'fayw': 1, 'fay': 1, 'faw': 1, 'fat': 1, 'faqh': 1, 'fap': 2, 'fan': 1, 'fam': 1, 'fab': 2, 'fa': 7, 'dzym': 1, 'dzxb': 1, 'dzw': 2, 'dzp': 1, 'dzm': 1, 'dzki': 1, 'dzg': 1, 'dzcc': 1, 'dzb': 1, 'dz': 6, 'dyzc': 1, 'dyzb': 1, 'dyy': 1, 'dywq': 1, 'dywh': 1, 'dywb': 1, 'dytj': 1, 'dytc': 1, 'dyt': 2, 'dyq': 1, 'dymw': 1, 'dym': 1, 'dykz': 1, 'dyk': 1, 'dyiq': 1, 'dyi': 1, 'dyhm': 1, 'dygp': 1, 'dyga': 1, 'dyfb': 1, 'dycz': 1, 'dycn': 1, 'dych': 1, 'dyc': 1, 'dyb': 1, 'dya': 1, 'dy': 7, 'dxzc': 1, 'dxyb': 1, 'dxy': 2, 'dxwp': 1, 'dxw': 2, 'dxv': 2, 'dxnc': 1, 'dxj': 1, 'dxhj': 1, 'dxfn': 1, 'dxcf': 1, 'dxby': 1, 'dxbt': 1, 'dxbc': 1, 'dx': 7, 'dwz': 1, 'dwy': 1, 'dwx': 3, 'dwqb': 1, 'dwq': 2, 'dwp': 1, 'dwm': 2, 'dwkh': 1, 'dwj': 1, 'dwhm': 1, 'dwh': 1, 'dwgm': 1, 'dwgc': 1, 'dwfk': 1, 'dwcj': 1, 'dwc': 1, 'dwbz': 1, 'dwbp': 1, 'dwbf': 1, 'dwb': 1, 'dw': 13, 'dvz': 1, 'dvw': 1, 'dvta': 1, 'dvq': 1, 'dvpb': 1, 'dvn': 1, 'dvk': 1, 'dvjz': 1, 'dvjk': 1, 'dviw': 1, 'dvf': 1, 'dvcm': 1, 'dvch': 1, 'dvcb': 1, 'dvc': 1, 'dva': 1, 'dv': 5, 'dtyi': 1, 'dtw': 1, 'dtvb': 1, 'dtki': 1, 'dthy': 2, 'dthx': 1, 'dth': 2, 'dtcn': 1, 'dtc': 1, 'dt': 6, 'dqyy': 1, 'dqv': 2, 'dqpm': 1, 'dqh': 1, 'dqfb': 1, 'dqf': 1, 'dqc': 1, 'dqbc': 1, 'dqb': 1, 'dq': 8, 'dpzm': 1, 'dpw': 1, 'dpt': 1, 'dpq': 1, 'dphx': 1, 'dpgf': 1, 'dpf': 2, 'dpc': 2, 'dpbv': 1, 'dpbc': 1, 'dp': 10, 'dnyg': 2, 'dnxt': 1, 'dnxc': 1, 'dnwp': 1, 'dnwa': 1, 'dnw': 2, 'dnq': 1, 'dnpx': 1, 'dnp': 2, 'dnjw': 1, 'dniy': 1, 'dnh': 2, 'dngy': 1, 'dng': 1, 'dnbq': 1, 'dnac': 1, 'dn': 8, 'dmzy': 1, 'dmy': 1, 'dmw': 1, 'dmqy': 1, 'dmpt': 1, 'dmk': 1, 'dmiw': 1, 'dmip': 1, 'dmi': 1, 'dmhb': 1, 'dmh': 1, 'dmf': 1, 'dmcg': 1, 'dmbg': 1, 'dmb': 1, 'dm': 2, 'dkwz': 1, 'dkw': 1, 'dkqt': 1, 'dkqn': 1, 'dkqc': 1, 'dkqb': 1, 'dkp': 1, 'dkn': 1, 'dkm': 1, 'dkjw': 1, 'dkiq': 1, 'dkij': 1, 'dkcy': 1, 'dkca': 1, 'dkb': 1, 'dka': 2, 'dk': 4, 'djyv': 1, 'djwa': 1, 'djv': 2, 'djtb': 1, 'djt': 1, 'djq': 1, 'dji': 1, 'djfy': 1, 'djf': 1, 'djc': 1, 'djb': 1, 'djav': 1, 'dj': 4, 'diw': 2, 'dinp': 1, 'dij': 1, 'dig': 1, 'difj': 1, 'dic': 1, 'diby': 1, 'diay': 1, 'dia': 1, 'di': 4, 'dhzw': 1, 'dhy': 1, 'dhx': 1, 'dhwk': 1, 'dhw': 1, 'dhtb': 1, 'dhp': 1, 'dhm': 1, 'dhj': 1, 'dhi': 1, 'dhf': 1, 'dhcf': 1, 'dhc': 1, 'dhbi': 1, 'dhb': 1, 'dh': 15, 'dgyw': 1, 'dgy': 1, 'dgwc': 1, 'dgpi': 1, 'dgnt': 1, 'dgn': 1, 'dgmy': 1, 'dgmc': 1, 'dgm': 1, 'dgh': 1, 'dgcv': 1, 'dgci': 1, 'dgbc': 1, 'dgb': 2, 'dga': 1, 'dg': 5, 'dfzx': 1, 'dfw': 1, 'dfc': 1, 'dfby': 1, 'dfba': 1, 'dfb': 1, 'df': 8, 'dczy': 1, 'dcyp': 1, 'dcy': 1, 'dcxm': 1, 'dcw': 1, 'dctw': 1, 'dcnb': 1, 'dcm': 1, 'dckh': 1, 'dck': 1, 'dcjk': 1, 'dcjb': 1, 'dcig': 1, 'dchb': 1, 'dch': 1, 'dcfb': 1, 'dccy': 1, 'dcbb': 1, 'dcb': 2, 'dc': 14, 'dby': 3, 'dbx': 2, 'dbw': 3, 'dbv': 2, 'dbtf': 1, 'dbt': 2, 'dbqx': 1, 'dbqw': 1, 'dbqp': 2, 'dbp': 1, 'dbn': 1, 'dbkv': 1, 'dbk': 1, 'dbjf': 1, 'dbjc': 1, 'dbik': 1, 'dbi': 2, 'dbhv': 1, 'dbgv': 1, 'dbct': 1, 'dbci': 1, 'dbc': 1, 'dbbz': 1, 'dbbw': 1, 'dbbv': 1, 'dbbp': 1, 'dbbm': 1, 'dbbj': 1, 'dbbg': 1, 'dbb': 2, 'dbap': 1, 'dba': 2, 'db': 13, 'daz': 1, 'dax': 1, 'davy': 1, 'dam': 1, 'dajk': 1, 'dag': 1, 'dab': 3, 'da': 4, 'czx': 1, 'czwy': 1, 'czw': 1, 'cznp': 1, 'czn': 1, 'czkq': 1, 'czkf': 1, 'czkd': 1, 'czjf': 1, 'czi': 1, 'czh': 3, 'czd': 1, 'czcd': 1, 'czbx': 2, 'czbq': 1, 'czbp': 1, 'czbk': 1, 'czbg': 1, 'czbf': 1, 'czb': 2, 'cz': 12, 'cyzw': 1, 'cyy': 2, 'cyxd': 1, 'cyxb': 1, 'cywx': 1, 'cyw': 1, 'cyv': 1, 'cyqm': 1, 'cyqh': 2, 'cypt': 1, 'cyph': 1, 'cyp': 2, 'cyn': 2, 'cymz': 2, 'cym': 1, 'cykh': 1, 'cyji': 1, 'cyip': 1, 'cyia': 1, 'cyhc': 1, 'cyh': 5, 'cygb': 1, 'cyfp': 1, 'cydw': 1, 'cybz': 1, 'cybm': 1, 'cybi': 1, 'cyb': 7, 'cyaz': 1, 'cyat': 1, 'cyag': 1, 'cya': 2, 'cy': 27, 'cxzy': 1, 'cxzm': 1, 'cxz': 1, 'cxy': 1, 'cxwb': 1, 'cxw': 1, 'cxv': 2, 'cxtw': 1, 'cxtd': 1, 'cxqb': 1, 'cxp': 1, 'cxn': 1, 'cxmk': 1, 'cxmg': 1, 'cxm': 2, 'cxhf': 1, 'cxgh': 1, 'cxg': 1, 'cxfq': 1, 'cxfn': 1, 'cxf': 2, 'cxd': 2, 'cxc': 1, 'cxb': 1, 'cxaw': 1, 'cx': 4, 'cwzf': 1, 'cwy': 3, 'cwxm': 1, 'cwxb': 2, 'cwwy': 1, 'cwwp': 1, 'cww': 5, 'cwvx': 1, 'cwvb': 1, 'cwv': 1, 'cwtb': 2, 'cwqf': 1, 'cwqa': 1, 'cwpt': 1, 'cwpb': 1, 'cwp': 2, 'cwny': 1, 'cwng': 1, 'cwnd': 1, 'cwn': 1, 'cwkd': 1, 'cwji': 1, 'cwjh': 1, 'cwj': 1, 'cwib': 1, 'cwht': 1, 'cwh': 2, 'cwgn': 1, 'cwg': 1, 'cwfz': 1, 'cwf': 2, 'cwdb': 1, 'cwd': 1, 'cwcy': 1, 'cwc': 1, 'cwbt': 1, 'cwbm': 1, 'cwbi': 1, 'cwb': 2, 'cwax': 2, 'cwat': 1, 'cw': 17, 'cvzp': 1, 'cvyw': 1, 'cvxw': 1, 'cvxi': 1, 'cvx': 1, 'cvwn': 1, 'cvt': 1, 'cvij': 1, 'cvhc': 1, 'cvh': 1, 'cvd': 1, 'cvct': 1, 'cvc': 1, 'cvbh': 1, 'cvbd': 1, 'cvb': 4, 'cv': 9, 'ctz': 1, 'ctwx': 1, 'ctvw': 1, 'ctpn': 1, 'ctpb': 1, 'ctp': 2, 'ctm': 1, 'ctk': 1, 'ctix': 1, 'cthy': 1, 'cthn': 1, 'cthb': 1, 'cth': 1, 'ctgm': 1, 'ctf': 1, 'ctbv': 1, 'ctbp': 1, 'ctbj': 1, 'ctb': 2, 'cta': 2, 'ct': 16, 'cqz': 1, 'cqy': 1, 'cqx': 1, 'cqwy': 1, 'cqw': 1, 'cqt': 2, 'cqnc': 1, 'cqnb': 1, 'cqkh': 1, 'cqk': 4, 'cqjc': 1, 'cqj': 2, 'cqi': 1, 'cqhb': 1, 'cqfa': 1, 'cqdj': 1, 'cqc': 1, 'cqbj': 1, 'cqbf': 1, 'cqbd': 1, 'cqb': 1, 'cqay': 1, 'cq': 13, 'cpzy': 1, 'cpzx': 1, 'cpz': 1, 'cpy': 1, 'cpxh': 1, 'cpwn': 1, 'cpwc': 1, 'cpwa': 1, 'cpw': 2, 'cpv': 1, 'cpth': 1, 'cpt': 1, 'cpmd': 1, 'cpm': 2, 'cpk': 1, 'cpjf': 1, 'cpiv': 1, 'cpid': 1, 'cphn': 1, 'cph': 1, 'cpfc': 1, 'cpdy': 1, 'cpdb': 1, 'cpcw': 1, 'cpbi': 1, 'cpbd': 1, 'cpb': 2, 'cpab': 1, 'cp': 11, 'cnyx': 1, 'cnyd': 1, 'cny': 1, 'cnxp': 1, 'cnxh': 1, 'cnx': 1, 'cnvy': 1, 'cnv': 1, 'cntz': 1, 'cntb': 1, 'cnq': 1, 'cnp': 2, 'cnm': 1, 'cnky': 1, 'cnhp': 1, 'cngw': 1, 'cngf': 1, 'cng': 1, 'cnbm': 2, 'cnbk': 1, 'cnb': 3, 'cn': 10, 'cmzf': 1, 'cmz': 1, 'cmyw': 1, 'cmyd': 1, 'cmx': 1, 'cmvw': 1, 'cmva': 1, 'cmtv': 1, 'cmt': 1, 'cmpj': 1, 'cmjb': 1, 'cmit': 1, 'cmik': 1, 'cmij': 1, 'cmgv': 1, 'cmg': 2, 'cmby': 2, 'cmbj': 1, 'cmbi': 1, 'cmb': 1, 'cma': 2, 'cm': 12, 'ckyh': 1, 'ckyb': 1, 'ckx': 1, 'ckqb': 1, 'cknz': 1, 'ckn': 1, 'ckmx': 1, 'ckmv': 1, 'ckmp': 1, 'ckhz': 1, 'ckhm': 1, 'ckhc': 1, 'ckh': 2, 'ckft': 1, 'ckf': 1, 'ckdx': 1, 'ckdj': 1, 'ckc': 1, 'ckbh': 1, 'ckbg': 1, 'ckbb': 1, 'ckb': 1, 'ckam': 1, 'ck': 10, 'cjy': 2, 'cjxw': 1, 'cjx': 2, 'cjt': 2, 'cjq': 1, 'cjp': 1, 'cjnm': 1, 'cjih': 1, 'cji': 1, 'cjha': 1, 'cjgw': 1, 'cjci': 1, 'cjbz': 1, 'cjbx': 1, 'cjba': 1, 'cjb': 2, 'cj': 6, 'cizb': 1, 'ciz': 1, 'ciy': 1, 'cix': 1, 'ciwy': 1, 'ciw': 2, 'civm': 1, 'cit': 2, 'ciqw': 1, 'cip': 2, 'cina': 1, 'cik': 1, 'cijh': 1, 'cihx': 1, 'cigc': 1, 'cifb': 1, 'cidn': 1, 'cid': 1, 'cicq': 1, 'ciby': 1, 'ci': 8, 'chzv': 1, 'chzd': 1, 'chz': 1, 'chyz': 1, 'chyw': 1, 'chym': 1, 'chyc': 1, 'chyb': 1, 'chy': 1, 'chx': 2, 'chwz': 1, 'chwi': 1, 'chwf': 1, 'chw': 3, 'chtm': 1, 'chtf': 1, 'chpn': 1, 'chp': 1, 'chn': 1, 'chk': 1, 'chjd': 1, 'chj': 2, 'chh': 3, 'chgv': 1, 'chf': 1, 'chdt': 1, 'chdi': 1, 'chd': 1, 'chbz': 1, 'chby': 1, 'chb': 5, 'chap': 1, 'cha': 1, 'ch': 22, 'cgz': 1, 'cgyw': 1, 'cgyt': 1, 'cgyc': 1, 'cgwt': 1, 'cgwi': 1, 'cgwh': 1, 'cgw': 1, 'cgvm': 1, 'cgti': 1, 'cgtf': 1, 'cgqi': 1, 'cgp': 1, 'cgnb': 1, 'cgm': 1, 'cgkf': 1, 'cgk': 1, 'cghy': 1, 'cgdw': 1, 'cgc': 1, 'cgby': 1, 'cgbv': 1, 'cgb': 1, 'cg': 11, 'cfz': 1, 'cfyi': 1, 'cfy': 1, 'cfvq': 1, 'cfva': 1, 'cfv': 1, 'cft': 1, 'cfqv': 1, 'cfq': 1, 'cfjw': 1, 'cfja': 1, 'cfj': 1, 'cfic': 1, 'cfi': 2, 'cfhx': 1, 'cfhd': 1, 'cfh': 1, 'cfd': 1, 'cfbk': 1, 'cfb': 1, 'cf': 15, 'cdzt': 1, 'cdyi': 1, 'cdy': 1, 'cdw': 2, 'cdv': 1, 'cdpi': 1, 'cdp': 1, 'cdn': 1, 'cdk': 1, 'cdjx': 1, 'cdjb': 1, 'cdj': 1, 'cdig': 1, 'cdh': 2, 'cdbg': 1, 'cdb': 3, 'cd': 14, 'ccz': 1, 'ccy': 1, 'ccw': 1, 'ccvy': 1, 'cctz': 1, 'cctx': 1, 'cctw': 1, 'ccqy': 1, 'ccq': 1, 'ccp': 2, 'ccmi': 1, 'ccmb': 1, 'ccj': 1, 'cchd': 1, 'cch': 2, 'ccg': 1, 'ccf': 1, 'ccdg': 1, 'ccb': 2, 'ccak': 1, 'cc': 9, 'cbzq': 1, 'cbzj': 1, 'cbz': 1, 'cbyt': 1, 'cbyk': 1, 'cbyi': 1, 'cbyg': 1, 'cbyb': 1, 'cby': 4, 'cbxp': 1, 'cbxh': 1, 'cbxc': 1, 'cbx': 1, 'cbwh': 1, 'cbw': 2, 'cbvi': 1, 'cbv': 1, 'cbth': 1, 'cbqc': 1, 'cbq': 2, 'cbpw': 1, 'cbpq': 1, 'cbpk': 1, 'cbpb': 1, 'cbp': 1, 'cbnw': 1, 'cbnp': 1, 'cbnc': 1, 'cbn': 4, 'cbmy': 1, 'cbmt': 1, 'cbmp': 1, 'cbmi': 1, 'cbmc': 1, 'cbm': 2, 'cbkw': 1, 'cbk': 1, 'cbjm': 1, 'cbhw': 1, 'cbhm': 2, 'cbh': 1, 'cbgw': 1, 'cbg': 2, 'cbfv': 1, 'cbf': 1, 'cbdx': 1, 'cbdw': 1, 'cbd': 4, 'cbcm': 1, 'cbc': 2, 'cbb': 5, 'cbaw': 1, 'cbad': 1, 'cba': 3, 'cb': 40, 'cayq': 1, 'cawh': 1, 'cawg': 2, 'caw': 1, 'cavm': 1, 'cat': 2, 'caq': 2, 'caph': 1, 'cap': 1, 'camh': 1, 'cam': 1, 'cakj': 1, 'cajm': 1, 'cajf': 1, 'cajc': 1, 'caj': 2, 'caif': 1, 'cahg': 1, 'cah': 3, 'cafi': 1, 'caf': 1, 'cadw': 1, 'cacn': 1, 'cacj': 1, 'cac': 1, 'cabk': 1, 'cabh': 1, 'cab': 2, 'ca': 15, 'bzyc': 1, 'bzy': 2, 'bzxf': 1, 'bzx': 2, 'bzwp': 1, 'bzwg': 1, 'bzqw': 1, 'bzny': 1, 'bznm': 1, 'bzn': 1, 'bzmw': 1, 'bzm': 1, 'bzkw': 1, 'bzj': 5, 'bzip': 1, 'bzhb': 1, 'bzh': 2, 'bzf': 1, 'bzdm': 1, 'bzch': 1, 'bzcd': 1, 'bzc': 4, 'bzb': 3, 'bzay': 1, 'bza': 2, 'bz': 21, 'byzc': 1, 'byyk': 1, 'byxt': 1, 'byx': 2, 'bywn': 1, 'bywh': 1, 'bywf': 2, 'bywd': 1, 'bywb': 1, 'byv': 1, 'byty': 1, 'byqw': 1, 'byq': 2, 'bypx': 1, 'bypk': 2, 'byp': 4, 'bynh': 2, 'bym': 4, 'byk': 2, 'byjw': 1, 'byjc': 1, 'byj': 2, 'byit': 1, 'byig': 1, 'byi': 3, 'byhz': 1, 'byhj': 1, 'byh': 1, 'byfp': 1, 'byfh': 1, 'byfa': 1, 'byf': 3, 'bydz': 1, 'bydp': 1, 'byd': 2, 'bycx': 1, 'bycw': 1, 'bycq': 1, 'bycm': 1, 'byc': 2, 'byby': 1, 'byb': 2, 'byaw': 1, 'byaf': 1, 'byac': 1, 'bya': 1, 'by': 43, 'bxzd': 1, 'bxyv': 1, 'bxyh': 1, 'bxyb': 1, 'bxy': 6, 'bxwy': 1, 'bxwp': 1, 'bxw': 4, 'bxtw': 1, 'bxqw': 1, 'bxq': 1, 'bxp': 2, 'bxny': 1, 'bxnh': 1, 'bxnb': 1, 'bxm': 1, 'bxj': 1, 'bxim': 1, 'bxic': 2, 'bxh': 1, 'bxgm': 1, 'bxf': 1, 'bxdk': 1, 'bxdg': 1, 'bxd': 2, 'bxcb': 1, 'bxc': 2, 'bxbh': 1, 'bxbd': 1, 'bxb': 1, 'bx': 17, 'bwzv': 1, 'bwza': 2, 'bwz': 2, 'bwyg': 2, 'bwyb': 1, 'bwy': 3, 'bwxv': 1, 'bwxh': 1, 'bwxg': 1, 'bwx': 3, 'bwwz': 1, 'bwwc': 1, 'bwwb': 1, 'bww': 1, 'bwvk': 1, 'bwv': 1, 'bwtv': 1, 'bwtk': 1, 'bwta': 1, 'bwt': 2, 'bwqz': 1, 'bwqg': 1, 'bwqc': 1, 'bwq': 1, 'bwpx': 1, 'bwph': 1, 'bwpc': 1, 'bwp': 1, 'bwnq': 1, 'bwn': 1, 'bwmp': 1, 'bwm': 1, 'bwkq': 1, 'bwkg': 1, 'bwkf': 1, 'bwk': 1, 'bwjc': 1, 'bwj': 1, 'bwiq': 1, 'bwim': 1, 'bwih': 1, 'bwif': 1, 'bwib': 1, 'bwhw': 1, 'bwhh': 1, 'bwhd': 1, 'bwh': 5, 'bwgb': 1, 'bwg': 2, 'bwfk': 1, 'bwfc': 1, 'bwfa': 1, 'bwf': 2, 'bwd': 1, 'bwcz': 2, 'bwcn': 1, 'bwcm': 1, 'bwch': 1, 'bwca': 1, 'bwc': 1, 'bwbm': 1, 'bwbc': 1, 'bwb': 2, 'bwab': 1, 'bwa': 2, 'bw': 30, 'bvz': 2, 'bvyp': 1, 'bvy': 1, 'bvxw': 1, 'bvxq': 1, 'bvx': 1, 'bvwk': 1, 'bvwa': 1, 'bvw': 2, 'bvpn': 1, 'bvpg': 1, 'bvp': 1, 'bvmf': 1, 'bvkj': 1, 'bvk': 1, 'bvjy': 1, 'bvj': 1, 'bvic': 1, 'bvhw': 1, 'bvg': 1, 'bvct': 1, 'bvcb': 1, 'bvc': 4, 'bvb': 3, 'bvai': 1, 'bv': 14, 'btyi': 1, 'btyc': 1, 'btx': 4, 'btwc': 1, 'btwa': 1, 'btw': 1, 'btv': 1, 'btq': 1, 'btp': 2, 'btnb': 1, 'btn': 1, 'btm': 1, 'btk': 1, 'btiz': 1, 'bti': 2, 'bthn': 1, 'bthm': 1, 'bthj': 1, 'bthg': 1, 'bth': 2, 'btf': 2, 'btda': 1, 'btd': 2, 'btcn': 1, 'btcd': 1, 'btc': 5, 'btbx': 2, 'btbw': 1, 'btbi': 1, 'btbd': 1, 'bta': 1, 'bt': 20, 'bqzw': 1, 'bqzb': 1, 'bqyp': 1, 'bqyk': 1, 'bqy': 2, 'bqxv': 1, 'bqwv': 1, 'bqwi': 1, 'bqwb': 1, 'bqw': 1, 'bqv': 1, 'bqtw': 1, 'bqt': 2, 'bqp': 2, 'bqnh': 1, 'bqn': 1, 'bqkb': 1, 'bqjv': 1, 'bqj': 1, 'bqi': 3, 'bqhv': 1, 'bqhk': 1, 'bqhb': 1, 'bqh': 4, 'bqg': 2, 'bqdm': 1, 'bqdi': 1, 'bqd': 2, 'bqcy': 1, 'bqcv': 1, 'bqch': 1, 'bqc': 1, 'bqbz': 1, 'bqbx': 1, 'bqbn': 1, 'bqbh': 1, 'bqb': 1, 'bqay': 1, 'bqa': 1, 'bq': 23, 'bpyf': 1, 'bpyd': 1, 'bpy': 2, 'bpx': 1, 'bpwx': 1, 'bpw': 1, 'bpvy': 1, 'bptw': 1, 'bpth': 1, 'bpky': 1, 'bpkc': 1, 'bpk': 2, 'bpjz': 1, 'bpib': 1, 'bpi': 1, 'bphy': 1, 'bphv': 1, 'bphj': 1, 'bphi': 1, 'bphb': 1, 'bph': 3, 'bpf': 1, 'bpd': 1, 'bpcv': 1, 'bpca': 1, 'bpbj': 1, 'bpbd': 1, 'bpb': 4, 'bp': 17, 'bnzv': 1, 'bnz': 1, 'bny': 2, 'bnxg': 1, 'bnxf': 1, 'bnxc': 1, 'bnwh': 1, 'bnwc': 1, 'bnw': 3, 'bnvf': 1, 'bnv': 1, 'bnt': 1, 'bnpk': 1, 'bnp': 1, 'bnkq': 1, 'bnjy': 1, 'bnj': 3, 'bni': 2, 'bnhh': 1, 'bnhc': 2, 'bnh': 1, 'bngh': 1, 'bnga': 1, 'bng': 1, 'bnfk': 1, 'bnf': 1, 'bnd': 2, 'bncc': 1, 'bnc': 1, 'bnby': 1, 'bnbj': 1, 'bnam': 1, 'bnac': 1, 'bna': 2, 'bn': 13, 'bmz': 2, 'bmy': 1, 'bmxh': 1, 'bmx': 1, 'bmw': 1, 'bmv': 1, 'bmtj': 1, 'bmtg': 1, 'bmt': 1, 'bmn': 1, 'bmkw': 1, 'bmk': 3, 'bmjy': 1, 'bmjt': 1, 'bmj': 3, 'bmig': 1, 'bmhp': 1, 'bmhh': 1, 'bmha': 1, 'bmh': 2, 'bmg': 2, 'bmfh': 1, 'bmf': 2, 'bmcz': 1, 'bmcy': 1, 'bmcd': 1, 'bmcb': 2, 'bmc': 2, 'bmbw': 1, 'bmbd': 1, 'bmbc': 1, 'bm': 12, 'bkzi': 1, 'bkyc': 1, 'bky': 1, 'bkxc': 1, 'bkwi': 1, 'bkwf': 1, 'bkwc': 1, 'bkwb': 1, 'bkw': 1, 'bkv': 2, 'bktf': 1, 'bkqm': 1, 'bkq': 1, 'bkj': 2, 'bkh': 1, 'bkgm': 1, 'bkg': 1, 'bkdh': 1, 'bkd': 2, 'bkcx': 1, 'bkc': 2, 'bkbv': 1, 'bkb': 3, 'bka': 1, 'bk': 18, 'bjz': 3, 'bjyi': 1, 'bjyb': 1, 'bjy': 1, 'bjwd': 1, 'bjw': 1, 'bjv': 1, 'bjtc': 1, 'bjt': 1, 'bjqw': 1, 'bjpi': 1, 'bjp': 2, 'bjn': 1, 'bjmk': 1, 'bjih': 1, 'bjha': 1, 'bjh': 2, 'bjg': 1, 'bjfc': 1, 'bjf': 2, 'bjdq': 1, 'bjcp': 1, 'bjc': 4, 'bjbk': 1, 'bjbc': 1, 'bjb': 2, 'bjac': 1, 'bj': 28, 'bizq': 1, 'biza': 1, 'biy': 1, 'bixy': 2, 'bixq': 1, 'bixc': 1, 'bix': 3, 'biwn': 1, 'biv': 2, 'biq': 1, 'bik': 1, 'bijw': 1, 'bijc': 1, 'bij': 2, 'bihp': 1, 'bihm': 1, 'bih': 2, 'bigw': 1, 'bigh': 1, 'bif': 1, 'bidx': 1, 'bidp': 1, 'bid': 2, 'bic': 2, 'bibt': 1, 'bib': 1, 'biad': 1, 'bia': 1, 'bi': 21, 'bhzw': 1, 'bhzc': 1, 'bhz': 2, 'bhym': 1, 'bhyi': 1, 'bhy': 2, 'bhxh': 1, 'bhxa': 1, 'bhx': 4, 'bhwj': 1, 'bhwc': 1, 'bhw': 6, 'bhv': 2, 'bhtp': 1, 'bhtj': 1, 'bhti': 1, 'bht': 2, 'bhq': 1, 'bhpy': 1, 'bhpm': 1, 'bhp': 1, 'bhnb': 1, 'bhn': 4, 'bhmw': 1, 'bhmn': 1, 'bhmf': 1, 'bhky': 1, 'bhkj': 1, 'bhk': 2, 'bhjw': 1, 'bhhy': 1, 'bhhc': 1, 'bhh': 2, 'bhgw': 1, 'bhg': 1, 'bhf': 2, 'bhd': 2, 'bhcp': 1, 'bhch': 2, 'bhcf': 1, 'bhc': 3, 'bhbb': 1, 'bhba': 1, 'bhb': 5, 'bh': 44, 'bgzk': 1, 'bgyk': 1, 'bgy': 1, 'bgxf': 1, 'bgxb': 1, 'bgwc': 1, 'bgw': 2, 'bgv': 1, 'bgt': 1, 'bgqw': 1, 'bgpj': 1, 'bgph': 1, 'bgny': 1, 'bgni': 1, 'bgnf': 1, 'bgnb': 1, 'bgn': 1, 'bgmh': 1, 'bgky': 1, 'bgkc': 1, 'bgk': 3, 'bghv': 1, 'bghc': 1, 'bgh': 1, 'bgdw': 1, 'bgdp': 1, 'bgcy': 2, 'bgcj': 1, 'bgch': 1, 'bgcf': 1, 'bgca': 1, 'bgc': 1, 'bgbq': 1, 'bgba': 1, 'bg': 13, 'bfzt': 1, 'bfz': 1, 'bfyz': 1, 'bfy': 5, 'bfx': 3, 'bfwn': 1, 'bfwj': 1, 'bfw': 1, 'bftn': 1, 'bft': 2, 'bfqy': 1, 'bfq': 1, 'bfp': 2, 'bfn': 1, 'bfk': 3, 'bfjb': 1, 'bfj': 1, 'bfhv': 1, 'bfhp': 1, 'bfh': 2, 'bfgy': 1, 'bfdv': 1, 'bfd': 2, 'bfcv': 1, 'bfcp': 1, 'bfc': 1, 'bfby': 1, 'bfbt': 1, 'bfbq': 1, 'bfb': 1, 'bfac': 1, 'bfa': 1, 'bf': 20, 'bdzk': 1, 'bdz': 2, 'bdyz': 1, 'bdym': 1, 'bdw': 2, 'bdvi': 1, 'bdvc': 1, 'bdv': 1, 'bdtb': 1, 'bdta': 1, 'bdp': 2, 'bdn': 1, 'bdm': 2, 'bdk': 1, 'bdi': 1, 'bdhp': 1, 'bdh': 3, 'bdgh': 1, 'bdf': 3, 'bdcn': 1, 'bdc': 1, 'bdby': 1, 'bdbn': 1, 'bdbh': 1, 'bdax': 1, 'bdac': 1, 'bd': 19, 'bczj': 1, 'bcyz': 1, 'bcyn': 1, 'bcyi': 1, 'bcy': 5, 'bcxd': 2, 'bcx': 2, 'bcwx': 1, 'bcwv': 1, 'bcwj': 1, 'bcwf': 1, 'bcw': 2, 'bcvy': 1, 'bcvw': 1, 'bct': 3, 'bcqh': 1, 'bcq': 2, 'bcpw': 1, 'bcph': 1, 'bcpc': 1, 'bcng': 2, 'bcn': 2, 'bcm': 1, 'bck': 2, 'bciy': 1, 'bcim': 1, 'bci': 2, 'bchw': 1, 'bchk': 1, 'bch': 3, 'bcgf': 1, 'bcgb': 1, 'bcg': 1, 'bcfa': 1, 'bcf': 2, 'bcdv': 1, 'bcd': 3, 'bccw': 2, 'bccq': 1, 'bcc': 1, 'bcbx': 1, 'bcb': 3, 'bcah': 1, 'bc': 37, 'bbzw': 1, 'bbzn': 1, 'bbzh': 1, 'bbz': 1, 'bbyz': 1, 'bbyn': 1, 'bbyf': 1, 'bbyc': 1, 'bbya': 1, 'bby': 3, 'bbx': 3, 'bbwz': 1, 'bbww': 1, 'bbwh': 1, 'bbwg': 1, 'bbwb': 1, 'bbw': 3, 'bbvc': 1, 'bbv': 1, 'bbtw': 1, 'bbtv': 1, 'bbt': 2, 'bbqw': 1, 'bbq': 1, 'bbpq': 1, 'bbpc': 1, 'bbp': 1, 'bbnw': 2, 'bbnh': 1, 'bbnc': 1, 'bbn': 2, 'bbmh': 1, 'bbm': 1, 'bbky': 1, 'bbkn': 1, 'bbkd': 1, 'bbkb': 1, 'bbk': 3, 'bbjp': 1, 'bbjh': 1, 'bbj': 1, 'bbi': 1, 'bbhy': 2, 'bbh': 2, 'bbgp': 1, 'bbgk': 1, 'bbg': 1, 'bbfp': 1, 'bbf': 2, 'bbdy': 3, 'bbda': 1, 'bbch': 1, 'bbc': 6, 'bbby': 1, 'bbb': 3, 'bbad': 1, 'bba': 2, 'bb': 36, 'baz': 2, 'bay': 2, 'baxt': 1, 'bawy': 1, 'bawk': 1, 'baw': 1, 'batd': 1, 'batc': 1, 'baqc': 1, 'baq': 1, 'banf': 1, 'bamj': 1, 'bakw': 1, 'bakc': 1, 'bak': 1, 'baji': 1, 'baj': 1, 'baiw': 1, 'baic': 1, 'bai': 1, 'bahv': 1, 'bahd': 1, 'bah': 2, 'bagc': 1, 'badq': 1, 'bacz': 1, 'bacx': 1, 'bacj': 1, 'bac': 1, 'babz': 1, 'babp': 1, 'bab': 1, 'ba': 10, 'azy': 1, 'azqv': 1, 'azq': 1, 'azpn': 1, 'azn': 1, 'azh': 1, 'azfc': 1, 'azf': 1, 'azc': 1, 'az': 3, 'ayy': 1, 'aywy': 1, 'aywq': 1, 'ayvg': 1, 'aytj': 1, 'ayti': 1, 'aytg': 1, 'ayqf': 1, 'aymk': 1, 'aymf': 1, 'aymd': 1, 'ayi': 1, 'ayg': 1, 'ayfz': 1, 'ayfy': 1, 'ayf': 1, 'ayd': 1, 'ayc': 1, 'ayb': 3, 'ay': 11, 'axzt': 1, 'axyv': 1, 'axwh': 2, 'axni': 1, 'axk': 1, 'axic': 1, 'axcp': 1, 'axbi': 1, 'axb': 1, 'ax': 6, 'awz': 2, 'awy': 1, 'awx': 1, 'awwx': 1, 'awwh': 1, 'aww': 1, 'awv': 1, 'awtw': 1, 'awt': 1, 'awp': 1, 'awmp': 1, 'awm': 1, 'awjf': 1, 'awh': 1, 'awg': 1, 'awfw': 1, 'awdh': 1, 'awdb': 1, 'awd': 1, 'awc': 1, 'awby': 2, 'awbp': 1, 'awb': 1, 'aw': 14, 'avyw': 1, 'avy': 1, 'avw': 1, 'avq': 1, 'avnb': 1, 'avmh': 1, 'avm': 1, 'avjw': 1, 'avhm': 1, 'avhj': 1, 'avhg': 1, 'avh': 1, 'avfp': 1, 'avdp': 1, 'avdm': 1, 'avd': 1, 'avbn': 1, 'av': 4, 'atyy': 1, 'atyb': 1, 'atwv': 1, 'atwm': 1, 'atwb': 1, 'atqj': 1, 'atpj': 1, 'atnj': 1, 'atm': 2, 'atj': 1, 'ati': 1, 'at': 7, 'aqzw': 1, 'aqy': 1, 'aqt': 1, 'aqp': 1, 'aqjc': 1, 'aqf': 1, 'aqc': 2, 'aqb': 2, 'aq': 6, 'apyb': 1, 'apv': 1, 'apq': 2, 'apj': 1, 'apiw': 1, 'aphz': 1, 'apg': 1, 'apft': 1, 'apf': 1, 'apc': 1, 'apb': 1, 'ap': 1, 'anxt': 1, 'anxi': 1, 'anwz': 1, 'anqp': 1, 'anj': 1, 'anib': 1, 'anhp': 1, 'anhc': 1, 'anfc': 1, 'and': 1, 'an': 9, 'amyf': 1, 'amyd': 1, 'amwn': 1, 'amwh': 1, 'amth': 1, 'amq': 1, 'amnv': 1, 'amfq': 1, 'amdk': 2, 'ambz': 1, 'amb': 1, 'am': 2, 'aky': 2, 'akvw': 1, 'akv': 1, 'aktb': 1, 'akp': 1, 'akm': 1, 'akiq': 1, 'akf': 1, 'akdi': 1, 'akc': 1, 'akbi': 1, 'ak': 7, 'ajzc': 2, 'ajyw': 1, 'ajy': 1, 'ajxw': 1, 'ajwh': 1, 'ajw': 1, 'ajmy': 1, 'ajm': 1, 'ajkx': 1, 'ajh': 1, 'ajgv': 1, 'ajb': 1, 'aj': 1, 'aiyn': 1, 'aiyc': 1, 'aiy': 1, 'aix': 2, 'aiwy': 1, 'aivw': 1, 'aith': 1, 'aij': 1, 'aigk': 2, 'aif': 2, 'aibz': 1, 'aibm': 1, 'ai': 2, 'ahyv': 1, 'ahwb': 1, 'ahw': 2, 'ahvx': 1, 'ahvi': 1, 'ahv': 1, 'aht': 2, 'ahqf': 1, 'ahq': 1, 'ahpm': 1, 'ahp': 2, 'ahh': 1, 'ahg': 1, 'ahf': 2, 'ahdw': 1, 'ahd': 1, 'ahcq': 1, 'ahbd': 1, 'ah': 14, 'agzv': 1, 'agxn': 1, 'agvj': 1, 'agvb': 1, 'agt': 1, 'agp': 1, 'agin': 1, 'agib': 1, 'agh': 1, 'agfb': 1, 'agd': 1, 'ag': 8, 'afy': 1, 'afxg': 1, 'afvq': 1, 'afnj': 1, 'afmv': 1, 'afmk': 1, 'afj': 1, 'afhj': 1, 'afd': 1, 'afbc': 1, 'afb': 1, 'af': 11, 'ady': 1, 'advw': 1, 'adp': 1, 'admh': 1, 'adk': 1, 'adi': 1, 'adg': 1, 'adfn': 1, 'adcy': 1, 'adc': 1, 'adbm': 1, 'adbc': 1, 'ad': 6, 'aczy': 1, 'acyz': 1, 'acyw': 1, 'acyq': 1, 'acy': 1, 'acxj': 1, 'acxh': 1, 'acx': 2, 'acv': 1, 'acth': 1, 'acqv': 1, 'acq': 1, 'acpw': 1, 'acmy': 1, 'ackd': 1, 'ack': 1, 'acjx': 1, 'acig': 1, 'achb': 1, 'ach': 2, 'acgz': 1, 'acg': 1, 'acfi': 1, 'acdn': 1, 'acdb': 1, 'acc': 1, 'acbb': 1, 'acb': 1, 'ac': 9, 'abzg': 1, 'abzb': 1, 'abyb': 1, 'aby': 2, 'abxm': 1, 'abx': 1, 'abw': 1, 'abvb': 1, 'abty': 1, 'abti': 1, 'abqm': 1, 'abpg': 1, 'abpb': 1, 'abp': 3, 'abnw': 1, 'abmh': 1, 'abjz': 1, 'abh': 2, 'abgx': 1, 'abdz': 1, 'abd': 1, 'abcz': 1, 'abcw': 1, 'abc': 1, 'abbq': 1, 'ab': 16}





    ['bh',
     'by',
     'wb',
     'yb',
     'cb',
     'bc',
     'bb',
     'yw',
     'bw',
     'hb',
     'hy',
     'wc',
     'yc',
     'bj',
     'cy',
     'bq',
     'fb',
     'qb',
     'wy',
     'ch',
     'tb',
     'yn',
     'bi',
     'bz',
     'hw',
     'wh',
     'bf',
     'bt',
     'gb',
     'ib',
     'ty',
     'yz',
     'bd',
     'tc',
     'yh',
     'bk',
     'gw',
     'jy',
     'wx',
     'xb',
     'xc',
     'zh',
     'bp',
     'bx',
     'cw',
     'fh',
     'hc',
     'hj',
     'iy',
     'jb',
     'kb',
     'wp',
     'ab',
     'ct',
     'gh',
     'hm',
     'pb',
     'zb',
     'ca',
     'cf',
     'dh',
     'fc',
     'hi',
     'kw',
     'mb',
     'mw',
     'nb',
     'pc',
     'py',
     'vb',
     'wz',
     'yi',
     'yq',
     'yv',
     'ah',
     'aw',
     'bv',
     'cd',
     'dc',
     'hd',
     'hh',
     'ht',
     'hz',
     'jc',
     'jh',
     'mt',
     'my',
     'ny',
     'qc',
     'tn',
     'wg',
     'wm',
     'zc',
     'bg',
     'bn',
     'cq',
     'db',
     'dw',
     'fw',
     'fy',
     'kc',
     'qy',
     'vc',
     'ww',
     'yj',
     'za',
     'bm',
     'cm',
     'cz',
     'gx',
     'hk',
     'jv',
     'kn',
     'nh',
     'nv',
     'vw',
     'vy',
     'wd',
     'wn',
     'xh',
     'yd',
     'yf',
     'yy',
     'af',
     'ay',
     'cg',
     'cp',
     'fj',
     'gy',
     'hg',
     'ig',
     'in',
     'ja',
     'jd',
     'mc',
     'mh',
     'nq',
     'ph',
     'px',
     'qh',
     'th',
     'wa',
     'wi',
     'wj',
     'wk',
     'wq',
     'wt',
     'xw',
     'yp',
     'zy',
     'ba',
     'ck',
     'cn',
     'dp',
     'fk',
     'gc',
     'gt',
     'hf',
     'hp',
     'ih',
     'jw',
     'ki',
     'ky',
     'pw',
     'vg',
     'xg',
     'ym',
     'yt',
     'zn',
     'ac',
     'an',
     'cc',
     'cv',
     'gm',
     'hn',
     'hq',
     'hv',
     'ji',
     'kj',
     'kx',
     'mi',
     'nc',
     'nw',
     'pm',
     'pz',
     'qd',
     'qm',
     'qw',
     'vh',
     'vt',
     'wv',
     'xt',
     'xy',
     'zt',
     'zw',
     'ag',
     'ci',
     'df',
     'dn',
     'dq',
     'fd',
     'fm',
     'gj',
     'gq',
     'ha',
     'ic',
     'id',
     'im',
     'iw',
     'kd',
     'mn',
     'nf',
     'nj',
     'nt',
     'nx',
     'qj',
     'qp',
     'tq',
     'vx',
     'wf',
     'xk',
     'ya',
     'yg',
     'yk',
     'zk',
     'zp',
     'ak',
     'at',
     'cyb',
     'dx',
     'dy',
     'fa',
     'ga',
     'gd',
     'gi',
     'gk',
     'gp',
     'hx',
     'ix',
     'jn',
     'jp',
     'jt',
     'ka',
     'km',
     'ma',
     'md',
     'mx',
     'nk',
     'pn',
     'pq',
     'pv',
     'qi',
     'qv',
     'tg',
     'tw',
     'tz',
     'vhb',
     'vj',
     'wcb',
     'xm',
     'zm',
     'zq',
     'zx',
     'ad',
     'aq',
     'ax',
     'bbc',
     'bhw',
     'bxy',
     'cj',
     'dt',
     'dz',
     'fq',
     'hbm',
     'if',
     'ik',
     'iv',
     'jm',
     'jx',
     'kg',
     'kz',
     'mbb',
     'mf',
     'mv',
     'pa',
     'pf',
     'qa',
     'qg',
     'qk',
     'qt',
     'ta',
     'td',
     'tj',
     'tx',
     'vd',
     'vz',
     'xa',
     'ybh',
     'yx',
     'zcb',
     'zv',
     'bcy',
     'bfy',
     'bhb',
     'btc',
     'bwh',
     'bzj',
     'cbb',
     'chb',
     'cww',
     'cyh',
     'dg',
     'dv',
     'fi',
     'fn',
     'fp',
     'fz',
     'gbc',
     'gf',
     'gv',
     'gz',
     'hmb',
     'hym',
     'ij',
     'jf',
     'jz',
     'kh',
     'kp',
     'kv',
     'mg',
     'mj',
     'mq',
     'na',
     'nhb',
     'pk',
     'qf',
     'qz',
     'tm',
     'tp',
     'vp',
     'vq',
     'wcg',
     'wyb',
     'xd',
     'xi',
     'xn',
     'xp',
     'xq',
     'xz',
     'zd',
     'zg',
     'zhw',
     'av',
     'bhn',
     'bhx',
     'bjc',
     'bpb',
     'bqh',
     'btx',
     'bvc',
     'bxw',
     'bym',
     'byp',
     'bzc',
     'cbd',
     'cbn',
     'cby',
     'cqk',
     'cvb',
     'cx',
     'da',
     'di',
     'dj',
     'dk',
     'fg',
     'ft',
     'fv',
     'fx',
     'gib',
     'gn',
     'gyc',
     'hbc',
     'hbk',
     'hcb',
     'hdw',
     'hfh',
     'hgb',
     'hjb',
     'hqb',
     'hwy',
     'hxb',
     'ia',
     'ihb',
     'ip',
     'iq',
     'iz',
     'jbb',
     'jk',
     'jnq',
     'jq',
     'kbc',
     'kf',
     'kq',
     'mby',
     'mk',
     'nd',
     'ni',
     'nm',
     'nwb',
     'pi',
     'pj',
     'pwb',
     'qhb',
     'tv',
     'va',
     'vbw',
     'vi',
     'vk',
     'wbh',
     'wbj',
     'wcf',
     'whb',
     'wnc',
     'wpb',
     'xf',
     'xv',
     'xyb',
     'ybi',
     'ybj',
     'ybw',
     'ybx',
     'zi',
     'zj',
     'abp',
     'ayb',
     'az',
     'bbb',
     'bbdy',
     'bbk',
     'bbw',
     'bbx',
     'bby',
     'bcb',
     'bcd',
     'bch',
     'bct',
     'bdf',
     'bdh',
     'bfk',
     'bfx',
     'bgk',
     'bhc',
     'bix',
     'bjz',
     'bkb',
     'bmj',
     'bmk',
     'bnj',
     'bnw',
     'bph',
     'bqi',
     'bvb',
     'bwx',
     'bwy',
     'byf',
     'byi',
     'bzb',
     'cah',
     'cba',
     'cdb',
     'chh',
     'chw',
     'cnb',
     'cwy',
     'czh',
     'dab',
     'dbw',
     'dby',
     'dwx',
     'fkh',
     'fwh',
     'fyp',
     'fyv',
     'fyw',
     'gbh',
     'gbw',
     'gky',
     'gwc',
     'hab',
     'hbg',
     'hbgq',
     'hbh',
     'hby',
     'hcn',
     'hct',
     'hcw',
     'hgp',
     'hhc',
     'hib',
     'hiw',
     'hmj',
     'hnj',
     'hpc',
     'hvb',
     'hwc',
     'hwv',
     'hww',
     'hzb',
     'ibj',
     'ibz',
     'icb',
     'icf',
     'ifh',
     'igb',
     'igd',
     'ihk',
     'it',
     'ity',
     'iyc',
     'iyz',
     'jbw',
     'jfb',
     'jpd',
     'jyc',
     'jzw',
     'kbw',
     'kt',
     'kyb',
     'mnw',
     'mp',
     'mpb',
     'mwc',
     'mwk',
     'nbb',
     'nbp',
     'ncb',
     'nch',
     'ng',
     'nhy',
     'nvt',
     'nyq',
     'nzw',
     'pbn',
     'pcb',
     'pd',
     'pg',
     'phh',
     'phw',
     'piw',
     'qbc',
     'qbd',
     'qbn',
     'qn',
     'qwb',
     'qwy',
     'qx',
     'qyw',
     'tbb',
     'tbf',
     'tbx',
     'tcc',
     'tf',
     'tgc',
     'tk',
     'tpb',
     'twk',
     'tyw',
     'vch',
     'vgh',
     'vha',
     'vkb',
     'vn',
     'vpb',
     'vpm',
     'vtb',
     'vyb',
     'vyw',
     'wab',
     'wbt',
     'wca',
     'wfb',
     'wfd',
     'wgc',
     'whh',
     'wht',
     'wib',
     'wkc',
     'wkj',
     'wng',
     'wnx',
     'wny',
     'wqb',
     'wth',
     'wvh',
     'wym',
     'wyw',
     'xbq',
     'xby',
     'xcb',
     'xhw',
     'xnb',
     'xqb',
     'yaz',
     'ycj',
     'ycn',
     'ydh',
     'yhb',
     'yik',
     'yjw',
     'ykc',
     'yxi',
     'yych',
     'zdc',
     'zpc',
     'abh',
     'aby',
     'ach',
     'acx',
     'ahf',
     'ahp',
     'aht',
     'ahw',
     'ai',
     'aif',
     'aigk',
     'aix',
     'ajzc',
     'aky',
     'am',
     'amdk',
     'apq',
     'aqb',
     'aqc',
     'atm',
     'awby',
     'awz',
     'axwh',
     'bah',
     'bay',
     'baz',
     'bba',
     'bbf',
     'bbh',
     'bbhy',
     'bbn',
     'bbnw',
     'bbt',
     'bccw',
     'bcf',
     'bci',
     'bck',
     'bcn',
     'bcng',
     'bcq',
     'bcw',
     'bcx',
     'bcxd',
     'bdm',
     'bdp',
     'bdw',
     'bdz',
     'bfd',
     'bfh',
     'bfp',
     'bft',
     'bgcy',
     'bgw',
     'bhch',
     'bhd',
     'bhf',
     'bhh',
     'bhk',
     'bht',
     'bhv',
     'bhy',
     'bhz',
     'bic',
     'bid',
     'bih',
     'bij',
     'biv',
     'bixy',
     'bjb',
     'bjf',
     'bjh',
     'bjp',
     'bkc',
     'bkd',
     'bkj',
     'bkv',
     'bmc',
     'bmcb',
     'bmf',
     'bmg',
     'bmh',
     'bmz',
     'bna',
     'bnd',
     'bnhc',
     'bni',
     'bny',
     'bpk',
     'bpy',
     'bqd',
     'bqg',
     'bqp',
     'bqt',
     'bqy',
     'btbx',
     'btd',
     'btf',
     'bth',
     'bti',
     'btp',
     'bvw',
     'bvz',
     'bwa',
     'bwb',
     'bwcz',
     'bwf',
     'bwg',
     'bwt',
     'bwyg',
     'bwz',
     'bwza',
     'bxc',
     'bxd',
     'bxic',
     'bxp',
     'byb',
     'byc',
     'byd',
     'byj',
     'byk',
     'bynh',
     'bypk',
     'byq',
     'bywf',
     'byx',
     'bza',
     'bzh',
     'bzx',
     'bzy',
     'cab',
     'caj',
     'caq',
     'cat',
     'cawg',
     'cbc',
     'cbg',
     'cbhm',
     'cbm',
     'cbq',
     'cbw',
     'ccb',
     'cch',
     'ccp',
     'cdh',
     'cdw',
     'cfi',
     'chj',
     'chx',
     'cip',
     'cit',
     'ciw',
     'cjb',
     'cjt',
     'cjx',
     'cjy',
     'ckh',
     'cma',
     'cmby',
     'cmg',
     'cnbm',
     'cnp',
     'cpb',
     'cpm',
     'cpw',
     'cqj',
     'cqt',
     'cta',
     'ctb',
     'ctp',
     'cwax',
     'cwb',
     'cwf',
     'cwh',
     'cwp',
     'cwtb',
     'cwxb',
     'cxd',
     'cxf',
     'cxm',
     'cxv',
     'cya',
     'cymz',
     'cyn',
     'cyp',
     'cyqh',
     'cyy',
     'czb',
     'czbx',
     'dba',
     'dbb',
     'dbi',
     'dbqp',
     'dbt',
     'dbv',
     'dbx',
     'dcb',
     'dgb',
     'diw',
     'djv',
     'dka',
     'dm',
     'dnh',
     'dnp',
     'dnw',
     'dnyg',
     'dpc',
     'dpf',
     'dqv',
     'dth',
     'dthy',
     'dwm',
     'dwq',
     'dxv',
     'dxw',
     'dxy',
     'dyt',
     'dzw',
     'fab',
     'fap',
     'fbb',
     'fbh',
     'fbi',
     'fbw',
     'fby',
     'fcb',
     'fcby',
     'fcc',
     'fcg',
     'fdk',
     'fgy',
     'fhw',
     'fiv',
     'fjc',
     'fjq',
     'fmw',
     'fnc',
     'fpt',
     'fpw',
     'fpyc',
     'fqct',
     'ftm',
     'fty',
     'fxb',
     'fxqt',
     'fyb',
     'fyt',
     'fyz',
     'fzb',
     'gab',
     'gbb',
     'gbd',
     'gbj',
     'gbq',
     'gby',
     'gca',
     'gcb',
     'gck',
     'gcw',
     'gcz',
     'gdy',
     'gfh',
     'ghb',
     'ghm',
     'giy',
     'gkw',
     'gmk',
     'gqb',
     'gtb',
     'gty',
     'gvk',
     'gwb',
     'gwm',
     'gwy',
     'gwyb',
     'gxny',
     'gyh',
     'gyp',
     'gyx',
     'gzn',
     'hah',
     'hbp',
     'hbt',
     'hbv',
     'hca',
     'hcc',
     'hcd',
     'hch',
     'hcj',
     'hcv',
     'hcy',
     'hdbb',
     'hdbq',
     'hdyw',
     'hfb',
     'hfj',
     'hfp',
     'hfw',
     'hfy',
     'hgdy',
     'hhb',
     'hhn',
     'hij',
     'hjv',
     'hkb',
     'hmi',
     'hng',
     'hpbx',
     'hpy',
     'hpz',
     'hqg',
     'hqy',
     'hta',
     'htk',
     'htm',
     'htp',
     'hwb',
     'hwi',
     'hwp',
     'hyc',
     'hyh',
     'hyq',
     'hyt',
     'hyv',
     'hyw',
     'hzc',
     'hzq',
     'iab',
     'iba',
     'ibb',
     'ibc',
     'ibn',
     'icc',
     'icj',
     'icm',
     'icn',
     'ift',
     'ign',
     'igw',
     'iht',
     'ihz',
     'ijb',
     'ika',
     'imw',
     'ink',
     'iny',
     'ipb',
     'iqp',
     'iqy',
     'iwfb',
     'iwy',
     'ixm',
     'ixw',
     'iyb',
     'iyw',
     'iza',
     'izm',
     'jax',
     'jbh',
     'jbnb',
     'jbx',
     'jcd',
     'jct',
     'jcxb',
     'jdt',
     'jfc',
     'jfp',
     'jg',
     'jhb',
     'jhc',
     'jhdk',
     'jhh',
     'jhn',
     'jiq',
     'jnh',
     'jqp',
     'jqx',
     'jtv',
     'jvg',
     'jwf',
     'jwh',
     'jxw',
     'jyd',
     'jyq',
     'jyt',
     'jzc',
     'jzp',
     ...]




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
