<!--
 * @Author: Nettor
 * @Date: 2020-06-18 20:17:25
 * @LastEditors: Nettor
 * @LastEditTime: 2020-06-20 21:44:55
 * @Description: file content
-->

# Top K Frequent Elements

## Java Solution

```java
public class Solution {
   public List<Integer> topKFrequent(int[] nums, int k)
    {
        //step1—用哈希表统计数组中各元素出现的频次，表中“键”为元素数值，“值”为对应元素出现的频次
        Map<Integer,Integer> map=new HashMap<Integer,Integer>();
        for(int num:nums)//遍历数组
        {
            if(map.get(num)==null)//如果“键”为num的数据首次出现，则“值”设为1
                map.put(num, 1);
            else
                map.put(num, map.get(num)+1);//重复出现，则累计频次
        }

        //step2—桶排序
        List<Integer>[] bucket=new List[nums.length+1];//定义足够数量的桶
        for(int key:map.keySet())//按“键”遍历
        {
            int count=map.get(key);//获取数值为key的元素出现的频次
            //把出现频次相同的元素“扔”到序号等于频次的桶中
            if(bucket[count]==null)
               bucket[count]=new ArrayList<Integer>();
            bucket[count].add(key);
        }
        //step3—“逆序”取数据
        List<Integer> result=new ArrayList<Integer>();
        for(int i=nums.length;i>0;i--)//注意i的起始值，当数组只有一个数据时
        {
            if(bucket[i]!=null&&result.size()<k)
                result.addAll(bucket[i]);
        }
        return result;
    }
```

## Go Solution

```go
func topKFrequent(nums []int, k int) []int {
    set := make(map[int]int)
    ret := make([]int,0)
    //step.1 store the frequency in map,[key:numsElement,value:frequency]
    for _,value := range nums{
        if _,ok := set[value]; ok == true{
            set[value] += 1
        } else {
            set[value] = 1
        }
    }
    frequencyMap := make(map[int][]int)
    //step.2 reverse set,[key:frequency,value:numsElements]
    for key,value := range set{
        frequencyMap[value] = append(frequencyMap[value],key)
    }
    //step.3 sort
    //get keys from frequencyMap
    keys := make([]int,0)
    for k := range frequencyMap{
        keys = append(keys,k)
    }
    //sort keys
    sort.Ints(keys)
    //get value from frequencyMap by keys
    for i:=1;len(ret) < k;i++{
        ret = append(ret,frequencyMap[keys[len(keys)-i]]...)
    }
    return ret
}
```

- 连接两个数组在 append 最后一个数组后加三个点,arrry0 = append(array1,array2...)
- sort.Ints()可以对 int array 排序
