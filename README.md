# zhouql1978
It's my first GitHub project about PM2.5 data analysis.

pm为英文particulate matter的缩写，翻译成中文叫做颗粒物。pm2.5是指大气中直径小于或等于2.5微米的颗粒物，有时也被称作入肺颗粒物。我们日常常见的雾霾天气大 多数情况下就是由pm2.5造成的。虽然pm2.5在大气中的含量极少，但是，由于质量小，携带病毒及有害物质时间长、传输渠道多样、移动距离较远、对人 体和大家环境质量造成的危害较大，pm2.5的确是名符其实的隐型杀手!  



但是随着北京的大力治理，比如二氧化硫浓度降幅64.3%、淘汰167万辆老旧机动车、城六区整治千条背街小巷、原生垃圾实现无害化处理等等措施，北京PM2.5下降近四分之一, 我们终将赢得蓝天(来源北京娱乐信报)。


百度指数也给出了我们同样结论。数据从2013年1月到2018年6月，PM2.5指数明显在降低，也可以说是逐年减少。


了解了这些以后，我们切换到今天的主题，Python分析北京地区PM2.5。



环境



环境：MAC + Python3.6
IDE：Spyder
模块：matplotlib、pandas、numpy 、os、seaborn


数据集获取
    我们首先使用pandas包中read_csv文件读取数据集。该数据集包含2013年以来，美国大使馆和我国给出的每一个月的 PM 值。然后使用pandas包中基本数据方法进行数据预览，主要包括，数据集整体预览、前10行查看、数据文件的基本信息等。具体代码如下：




def collect_data():
    '''
        STEP1: 收集数据
    '''
    data_pd = pd.read_csv(os.path.join('./data/pm/Beijing_PM.csv'))

    return data_pd 


def inspect_data(data_pd):
    '''
        STEP2: 描述数据
    '''
    print('数据文件中一共有{}行，{}列'.format(data_pd.shape[0],data_pd.shape[1]))

    print('数据预览')
    print(data_pd)

    print('数据的前5行')
    print(data_pd.head)

    print('数据文件的的基本信息')
    print(data_pd.info())

    print('数据内容的统计信息')
    print(data_pd.describe())


运行结果截图如下


数据集共26280行，7列,占用1.4M内存，数据类型为int or float。


每年平均值分析


根据上面的数据集，首先分析的是每年的平均值，查看逐年变化情况。本次分析使用的技术点主要是分组，也就是根据年份（year）进行分组，然后使用柱形图进行可视化结果。从下面的两个图（PM chian mean vs PM us mean）可以看出，无论美国大使馆还是我国给出的PM数据，每年的PM值都在减少，说明我国治理卓有成效，终将获得蓝天。


为了更方便查看我国和美国使馆检测的每年平均PM2.5值的对比，我们可以使用python中的堆叠柱状图可视化，如下图所示。两个机构给出的数据没有很大的差异。具体的技术点主要是使用pandas包中的plot.bar函数，但是可视化之前需要先分组处理数据。具体代码：




#堆叠柱状图可视化
    filter_mean.plot.bar(stacked=True)
    plt.title('Mean PM 2.5')
    plt.tight_layout()
    plt.savefig(os.path.join(output_path,'filter_mean.png'))
    plt.show()

可视化结果如下


四季分析


四季分析主要是从四个季度进行对比分析。从下图可以看出，PM最高的季度是第四季，也就是冬天的时候雾霾最为严重。主要使用的技术点是使用箱形图分析。箱形图（Box-plot）又称为盒须图、盒式图或箱线图，是一种用作显示一组数据分散情况资料的统计图。




 #绘制盒形图函数boxplot
    sns.boxplot(x='season',y=var,data = data_df)
    plt.savefig(os.path.join(output_path,'year_'+var+'.png'))
    plt.show


运行效果如下：


每月以及每小时分析

从每月的分析看出，每年的11月、12月、1月最为严重。在每月里面，早晚高峰的时间更为严重。本次可视化的技术点是使用散点图。





def analyze_dual_variables(data_df,var1,var2):
    '''
        查看双变量的关系,散点图
    '''

    sns.jointplot(x=var1,y=var2,data=data_df)
    plt.savefig(os.path.join(output_path,var1+'_'+var2+'_sandian.png'))
    plt.show()
运行效果图


也可以使用透视表可视化每一个月的PM, 具体如下：



按小时分析

相关性分析

    相关性分析主要用到的技术点是热点图，以及相关性计算。利用热力图可以看数据表里多个特征两两的相似度，具体代码如下：




def analyze_variable_relationship(data_df):
    '''
        可视化变量关系
        热图
    '''
    #绘制出所有变量直接的关系，形成一个矩阵
    #corr计算相关系数
    corr_df = data_df.corr()
    #热图
    sns.heatmap(corr_df,annot=True)
    plt.savefig(os.path.join(output_path,'heatmap_df.png'))
    plt.show()

