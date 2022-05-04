```
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int i=0; // new array index
        for (int j=0; j<nums.size(); j++) {
            if (nums[j]!=val) {
                nums[i]=nums[j];
                i++;
            }
        } //j=2, i=1
        return i;
    }
};
```
