# raw count

# rpkm fpkm

# TPM | 适用于PCA和clustering

TPM不一定比rpkm好，TPM更类似于比例，fpkm更类似于含量。

假设总核苷酸数固定，当一些长度较长基因的拷贝数增加时，由于耗用核苷酸比较多，导致总数目变少，此时fpkm更接近于真实情况。

如果此时选用TPM，相当于总体乘以了一个大于1的因子，即会一定程度高估低表达基因

# CPM
