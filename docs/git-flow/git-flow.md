# Quy trình: 

![alt text][logo]

[logo]: ./git-flow.png "Git flow"

## Feature branch

    Ví dụ: 
    * Create Lease: ZP-1222 (ZP tương ứng với project name)
    - Tạo branch: ZP-1222 từ master
    - Fix bugs của create lease trên nhánh ZP-1222
    - ZP-1222 deploy lên nhánh develop( nhánh develop sẽ được đưa lên development server)
    - verified 
    - merge ZP-1222 to master (master sẽ được đánh tags sau mỗi lần được release, master coi là nhánh production)

## Related Feature Branch: 

    Ví dụ: 
    * Detail lease: ZP-1223
    - tạo Branch ZP-1223 từ ZP-1222
    - làm ZP-1223 
    - fix bugs trên ZP-1223
    - ZP-1223 deploy lên nhánh develop( nhánh develop sẽ được đưa lên development server)
    - verified 
    - merge ZP-1223 to master (master sẽ được đánh tags sau mỗi lần được release, master coi là nhánh production)

## Trong trường hợp cần một đoạn code từ một branch khác:

### features 2 depends features 1:

      A---B---C features 1
	 /         \
    D---E---F---G---H features 2

    Điều kiện: 
    - features 2 depends vào features 1 => muốn features 2 lên thì features 1 phải được lên trước
    Cách làm
    - git checkout ZP-1223 : chuyển sanh nhánh cần đoạn code nếu  feature 2 depend vào feature 1
    - git pull origin ZP-1222

### features 2 không depends features 2: 

     A---B---C features 1
              \
    D---E---F---G---H features 2
    
    Điều kiện: 
    - features 2 không depends vào features 1 và 2 đoạn code có thể được deploy riêng rẽ.
    - Đoạn code dùng chung phải được commit riêng
    Cách làm:
    - git cherry-pick <commit-hash>