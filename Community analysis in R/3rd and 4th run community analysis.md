---
title: "3rd_and_4th_combine_16S"
author: "Fang Liu"
date: "June 23, 2017"
output: pdf_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE,warning=FALSE)
```

### **Load library**


```{r}
library(metagenomeSeq)
library(phyloseq)
library(plyr)
library(dplyr)
library(scales)
library(RColorBrewer)
library(vegan)
library(ggplot2)
library(reshape2)
```

**Set up plot font type to Times New Toman**


```{r}
library(extrafont)
loadfonts(device = "win")
windowsFonts(Times=("TT Times New Roman"))
```


### Import OTU and taxonomy data to generate a phyloseq object


```{r}
# Set up work directory 

setwd("G:\\UT\\I_love_my_project\\3rd_and_4th_combine_run")
getwd()

# import mothur output

trial_phyloseq_origin<-import_mothur(mothur_shared_file = "trial.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.opti_mcc.unique_list.shared", mothur_constaxonomy_file ="trial.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.opti_mcc.unique_list.0.03.cons.taxonomy")

# Have a glance of original phyloseq objective
trial_phyloseq_origin #95991 OTU distributed across 20 samples

# Generatea meta data
trial_meta_df<-data.frame(sample_ID=sample_names(trial_phyloseq_origin),soil_type=as.factor(c(rep("Ag",10),rep("Forest",10))),treatment=as.factor(c(rep("Bulk",5),rep("Fresh",5),rep("Bulk",5),rep("Fresh",5))))
row.names(trial_meta_df)<-sample_names(trial_phyloseq_origin)
write.csv(trial_meta_df,"trial_meta.csv")


```


### Merge trial_phyloseq_origin with trial_meta object


```{r}
trial_meta<-sample_data(trial_meta_df)
identical(rownames(trial_meta_df),sample_names(trial_phyloseq_origin))
trial_phyloseq<-merge_phyloseq(trial_phyloseq_origin,trial_meta)
rank_names(trial_phyloseq)
colnames(tax_table(trial_phyloseq))<-c('Kingdom','Phylum','Class','Order','Family','Genus')
rank_names(trial_phyloseq)

```


### PCoA plot based on Raw_OTU count


```{r}

trial_phyloseq_bray<-vegdist(t(otu_table(trial_phyloseq)),method="bray",binary = FALSE)
trial_phyloseq_PCoA<-ordinate(trial_phyloseq,method = "PCoA", trial_phyloseq_bray)
plot_ordination(trial_phyloseq,trial_phyloseq_PCoA,type="samples",color="soil_type",shape = "treatment")+
  geom_point(size=4)+ggtitle("trial_phyloseq_weighted Bray-Curtis PCoA plot Axis1 and Axis2")+theme(title=(element_text(size=15,family = "Times",face="bold")),text=element_text(family = "Times",face="bold"),panel.border = element_rect(colour = "black", fill=NA, size=0.5),axis.text=element_text(size=13,family="Times"),
    axis.title=element_text(size = 15,face="bold",family = "Times"),legend.title = element_text(size=13,face="bold",family = "Times"),legend.text = (element_text(size=10,family = "Times")))
```


### Read depth distribution


```{r}
read_depth<-sample_sums(trial_phyloseq)
meta_combine<-data.frame(read_depth=read_depth,trial_meta_df)
ggplot(data=meta_combine,aes(x=sample_ID,y=read_depth))+geom_bar(stat='identity',fill='dark blue',width=0.6)+ggtitle('Read depth distribution')+theme(title=(element_text(size=15,family = "Times",face="bold")),text=element_text(family = "Times",face="bold"),panel.border = element_rect(colour = "black", fill=NA, size=0.5),axis.text=element_text(size=13,family="Times"),axis.text.x = element_text(angle=90,hjust = 1),
    axis.title=element_text(size = 15,face="bold",family = "Times"),legend.title = element_text(size=13,face="bold",family = "Times"),legend.text = (element_text(size=10,family = "Times")))
```


### Rarefaction plot


```{r}
write.csv(t(otu_table(trial_phyloseq)),file="trial_OTU.csv")
write.csv(otu_table(trial_phyloseq),file='trial_OTU_t.csv')
trial_otu_table<-read.csv("trial_OTU.csv",row.names = 1)
dim(trial_otu_table) # correct OTUs number
rare<-rarefy(trial_otu_table,sample = seq(0,max(sample_sums(trial_phyloseq)),by=100))
rownames(rare)<-rownames(trial_otu_table)
long_rare<-t(rare)
long_rare<-data.frame(subsample=seq(0,max(sample_sums(trial_phyloseq)),by=100),long_rare)
head(long_rare)
rare_melt<-melt(long_rare,id.vars = "subsample")
head(rare_melt)


# 1) Rarefaction curve using maximum read depth

ggplot(rare_melt,aes(x=subsample,y=value,colour=variable))+geom_point()+geom_line(linetype = "dashed")+theme(legend.position="top")+ylab("Bactria OTU numbers")+theme(title=(element_text(size=15,family = "Times",face="bold")),text=element_text(family = "Times",face="bold"),panel.border = element_rect(colour = "black", fill=NA, size=0.5),axis.text=element_text(size=13,family="Times"),
    axis.title=element_text(size = 15,face="bold",family = "Times"),legend.title = element_text(size=13,face="bold",family = "Times"),legend.text = (element_text(size=10,family = "Times")),legend.position="right")+ggtitle("Rarefaction curve with maximum read depth")

# 2) Rarefaction curve using minimum read depth

rare<-rarefy(trial_otu_table,sample = seq(0,min(sample_sums(trial_phyloseq)),by=100))
rownames(rare)<-rownames(trial_otu_table)
long_rare<-t(rare)
long_rare<-data.frame(subsample=seq(0,min(sample_sums(trial_phyloseq)),by=100),long_rare)
head(long_rare)
rare_melt<-melt(long_rare,id.vars = "subsample")
head(rare_melt)

ggplot(rare_melt,aes(x=subsample,y=value,colour=variable))+geom_point()+geom_line(linetype = "dashed")+theme(legend.position="top")+ylab("Bactria OTU numbers")+theme(title=(element_text(size=15,family = "Times",face="bold")),text=element_text(family = "Times",face="bold"),panel.border = element_rect(colour = "black", fill=NA, size=0.5),axis.text=element_text(size=13,family="Times"),
    axis.title=element_text(size = 15,face="bold",family = "Times"),legend.title = element_text(size=13,face="bold",family = "Times"),legend.text = (element_text(size=10,family = "Times")),legend.position="right")+ggtitle("Rarefaction curve with minimum read depth")

```



### Calculate diversity and barplot


```{r}
shannon<-diversity(trial_otu_table,index = 'shannon',MARGIN=1,base=exp(1))
simpson<-diversity(trial_otu_table,index='simpson',MARGIN = 1,base=exp(1))
OTU_num<-apply(otu_table(trial_phyloseq),2,function(x) sum(x>0))
diversity_combine<-data.frame(trial_meta_df,shannon=shannon,simpson=simpson,OTU_num=OTU_num)
head(diversity_combine)

# diversity barplot - Side by side grouped by factors
# https://stackoverflow.com/questions/22305023/how-to-get-a-barplot-with-several-variables-side-by-side-grouped-by-a-factor

diversity_combine<-data.frame(diversity_combine,group=paste(diversity_combine$soil_type,diversity_combine$treatment,sep = "_"))
diversity_melt<-melt(diversity_combine,id.vars = c("group"),measure.vars = c("shannon","simpson"))

ggplot(diversity_melt,aes(x=variable,y=value,fill=group))+
  stat_summary(fun.y = mean,geom="bar",position = position_dodge(1),width=0.8)+xlab("Diversity")+theme(title=(element_text(size=15,family = "Times",face="bold")),text=element_text(family = "Times",face="bold"),panel.border = element_rect(colour = "black", fill=NA, size=0.5),axis.text=element_text(size=13,family="Times"),
    axis.title=element_text(size = 15,face="bold",family = "Times"),legend.title = element_text(size=13,face="bold",family = "Times"),legend.text = (element_text(size=10,family = "Times")),legend.position="right")+ggtitle("Diversity barplot")


```


### Phylum level stacked barplot


```{r}
phylum_trial_phyloseq<-tax_glom(trial_phyloseq,taxrank = rank_names(trial_phyloseq)[2],NArm = TRUE) 
rank_names(trial_phyloseq)[2]
phylum_trial_phyloseq
taxa_names(phylum_trial_phyloseq)
tax_table(phylum_trial_phyloseq)
tax_table(phylum_trial_phyloseq)[,2]
# generate phylum_phyloseq with relative abundance
r_phylum_trial_phyloseq<-transform_sample_counts(phylum_trial_phyloseq,function(x) x/sum(x))
r_phylum_trial_phyloseq
# filter off phylum with relative abundance smaller than 0.01
prune_r_phylum_trial_phyloseq<-prune_taxa(taxa_sums(r_phylum_trial_phyloseq)>=0.01,r_phylum_trial_phyloseq)
prune_r_phylum_trial_phyloseq
tax_table(prune_r_phylum_trial_phyloseq)
final_phylum_phyloseq<-transform_sample_counts(prune_r_phylum_trial_phyloseq,function(x) x/sum(x))
sample_sums(final_phylum_phyloseq)

## melt phyloseq could use psmelt, but my compter don't have enough RAM for this, so I used below strategy.https://rdrr.io/bioc/phyloseq/man/psmelt.html

# write final_phylum_phyloseq to local computer in csv format
phylum_combine<-cbind(tax_table(final_phylum_phyloseq)[,2],otu_table(final_phylum_phyloseq))
write.csv(phylum_combine,file='phylum_combine.csv')
phylum_combine<-read.csv('phylum_combine.csv',row.names = 1)
phylum_melt<-melt(phylum_combine,id.vars = 'Phylum')
head(phylum_melt)


## Barplot

color<-c("#CBD588", "#5F7FC7", "orange","#DA5724", "#508578", "#CD9BCD",
   "#AD6F3B", "#673770","#D14285", "#652926", "#C84248", 
  "#8569D5", "#5E738F","#D1A33D", "#8A7C64", "#599861","#FFDEAD","#B2DFEE")

ggplot(phylum_melt, aes(x =variable , y = value,fill=Phylum))+geom_bar(stat='identity',colour="grey",size=0.1)+labs(x="Treatment",y="Relative Abundance")+scale_fill_manual(values=color)+theme(title=(element_text(size=15,family = "Times",face="bold")),axis.text.x = element_text(angle=90,hjust = 1),text=element_text(family = "Times",face="bold"),panel.border = element_rect(colour = "black", fill=NA, size=0.5),axis.text=element_text(size=13,family="Times"),
    axis.title=element_text(size = 15,face="bold",family = "Times"),legend.title = element_text(size=13,face="bold",family = "Times"),legend.text = (element_text(size=10,family = "Times")))

```


### Rarefaction- rarefy read depth to minimum read depth


```{r}
rarefied_trial_phyloseq<-rarefy_even_depth(trial_phyloseq,sample.size = min(sample_sums(trial_phyloseq)),rngseed = 1013,replace = TRUE,trimOTUs = TRUE,verbose = TRUE)
rarefied_trial_phyloseq  #55922 OTUs were removed because they are no longer present in any sample after random subsampling

write.csv(t(otu_table(rarefied_trial_phyloseq)),file = 'rarefied_OTU.csv')
write.csv(otu_table(rarefied_trial_phyloseq),file='rarefied_OTU_t.csv')

trial_phyloseq

# Rarefied OTU count based PCoA plot

rarefied_trial_phyloseq_bray<-vegdist(t(otu_table(rarefied_trial_phyloseq)),method="bray",binary = FALSE)
rarefied_trial_phyloseq_PCoA<-ordinate(rarefied_trial_phyloseq,method = "PCoA", rarefied_trial_phyloseq_bray)
plot_ordination(rarefied_trial_phyloseq,rarefied_trial_phyloseq_PCoA,type="samples",color="soil_type",shape = "treatment")+
  geom_point(size=4)+ggtitle("rarefied_trial_phyloseq_weighted Bray-Curtis PCoA plot Axis1 and Axis2")+theme(title=(element_text(size=15,family = "Times",face="bold")),text=element_text(family = "Times",face="bold"),panel.border = element_rect(colour = "black", fill=NA, size=0.5),axis.text=element_text(size=13,family="Times"),
    axis.title=element_text(size = 15,face="bold",family = "Times"),legend.title = element_text(size=13,face="bold",family = "Times"),legend.text = (element_text(size=10,family = "Times")))

```


### MetagenomeSeq normalization and differential abundance analysis -- Raw_OTU count based


```{r}

# Generate metagenomeSeq class object

trial_otu_metagenomeSeq<-loadMeta('G:\\UT\\I_love_my_project\\3rd_and_4th_combine_run\\trial_OTU_t.csv',sep = ',')
trial_meta_metagenomeSeq<-loadPhenoData('G:\\UT\\I_love_my_project\\3rd_and_4th_combine_run\\trial_meta.csv',tran = TRUE,sep=',')
trial_meta_metagenomeSeq<-AnnotatedDataFrame(trial_meta_metagenomeSeq)

trial_metagenomeSeq<-newMRexperiment(trial_otu_metagenomeSeq$counts,phenoData=trial_meta_metagenomeSeq)
exportMat(trial_metagenomeSeq,norm=TRUE,log=FALSE,sep=",",file="Normalized_trial_OTU.csv")


# differentail abundance analysis

# 1) comparation between Ag and forest soil type

norm_trial_metagenomeSeq<-cumNorm(trial_metagenomeSeq,p=cumNormStatFast(trial_metagenomeSeq))
mod<-model.matrix(~soil_type,data=trial_meta_df)
fit<-fitFeatureModel(norm_trial_metagenomeSeq,mod) # using zero inflated log normal model
ls(fit)
str(fit$fitZeroLogNormal)

MR_table<-MRtable(fit,by=2,number=1617,taxa = fit$taxa,uniqueNames = TRUE,adjustMethod = 'fdr',group=3)
MR_table_update<-MR_table[which(MR_table$adjPvalues<=0.05),]
dim(MR_table_update)
MR_table<-MRtable(fit,by=2,number=dim(MR_table_update)[1],taxa = fit$taxa,uniqueNames = TRUE,adjustMethod = 'fdr',group=3,file='MRtable_output.csv') # here group=3 means this table will be sorted by p_value in increasing way.


# 2) Compare between fresh and bulk soil

mod2<-model.matrix(~treatment,data=trial_meta_df)
fit2<-fitFeatureModel(norm_trial_metagenomeSeq,mod2) # using zero inflated log normal model
ls(fit2)
str(fit2$fitZeroLogNormal)
MR_table2<-MRtable(fit2,by=2,number=1000,taxa = fit$taxa,uniqueNames = TRUE,adjustMethod = 'fdr',group=3)
dim(MR_table2)
MR_table2_update<-MR_table2[which(MR_table2$adjPvalues<=0.05),]
dim(MR_table2_update)
MR_table2<-MRtable(fit2,by=2,number=dim(MR_table2_update)[1],taxa = fit2$taxa,uniqueNames = TRUE,adjustMethod = 'fdr',group=3,file='MRtable_output2.csv') # here group=3 means this table will be sorted by p_value in increasing way.

```


### MetagenomeSeq normalization and differential abundance analysis-- drarefied OTU count based


```{r}

trial_rarefied_otu_metagenomeSeq<-loadMeta('G:\\UT\\I_love_my_project\\3rd_and_4th_combine_run\\rarefied_OTU_t.csv',sep = ',')
trial_meta_metagenomeSeq<-loadPhenoData('G:\\UT\\I_love_my_project\\3rd_and_4th_combine_run\\trial_meta.csv',tran = TRUE,sep=',')
trial_meta_metagenomeSeq<-AnnotatedDataFrame(trial_meta_metagenomeSeq)

trial_rarefied_metagenomeSeq<-newMRexperiment(trial_rarefied_otu_metagenomeSeq$counts,phenoData=trial_meta_metagenomeSeq)
norm_trial_rarefied_metagenomeSeq<-cumNorm(trial_rarefied_metagenomeSeq,p=cumNormStatFast(trial_rarefied_metagenomeSeq))

mod_rarefied<-model.matrix(~soil_type,data=trial_meta_df)
fit_rarefied<-fitFeatureModel(norm_trial_rarefied_metagenomeSeq,mod_rarefied)
str(fit_rarefied$fitZeroLogNormal)

rarefied_MR_table<-MRtable(fit_rarefied,by=2,number=689,taxa = fit_rarefied$taxa,uniqueNames = TRUE,adjustMethod = 'fdr',group=3)
dim(rarefied_MR_table)
rarefied_MR_table_update<-rarefied_MR_table[which(rarefied_MR_table$adjPvalues<=0.05),]
dim(rarefied_MR_table_update)

rarefied_MR_table<-MRtable(fit_rarefied,by=2,number=dim(rarefied_MR_table_update)[1],taxa = fit_rarefied$taxa,uniqueNames = TRUE,adjustMethod = 'fdr',group=3,file='rarefied_MR_table.csv')


```



### Normalized OTU_tabel based phyloseq


```{r}

normalized_trial_otu<-read.csv('Normalized_trial_OTU.csv',row.names = 1,h=TRUE)
normalized_trial_otu<-otu_table(as.matrix(normalized_trial_otu),taxa_are_rows = TRUE)
normalized_trial_phyloseq<-phyloseq(normalized_trial_otu,tax_table(trial_phyloseq),sample_data(trial_phyloseq))
normalized_trial_phyloseq

# PCoA plot using normalized_trial_phyloseq


normalized_trial_phyloseq_bray<-vegdist(t(otu_table(normalized_trial_phyloseq)),method="bray",binary = FALSE)
normalized_trial_phyloseq_PCoA<-ordinate(normalized_trial_phyloseq,method = "PCoA", normalized_trial_phyloseq_bray)
plot_ordination(normalized_trial_phyloseq,normalized_trial_phyloseq_PCoA,type="samples",color="soil_type",shape = "treatment")+
  geom_point(size=4)+ggtitle("normalized_trial_phyloseq_weighted Bray-Curtis PCoA plot Axis1 and Axis2")+theme(title=(element_text(size=15,family = "Times",face="bold")),text=element_text(family = "Times",face="bold"),panel.border = element_rect(colour = "black", fill=NA, size=0.5),axis.text=element_text(size=13,family="Times"),
    axis.title=element_text(size = 15,face="bold",family = "Times"),legend.title = element_text(size=13,face="bold",family = "Times"),legend.text = (element_text(size=10,family = "Times")))



```

## Permanova test

```{r}
# Using original OTU_table
adonis(trial_otu_table~soil_type+treatment+read_depth,data=meta_combine,permutations = 100)

# Using rarefied OTU_table
rarefied_OTU<-read.csv('rarefied_OTU.csv',row.names = 1)
adonis(rarefied_OTU~soil_type+treatment,data=trial_meta_df,permutations = 100)

# Using normalized OTU_table
normalized_OTU<-read.csv('Normalized_trial_OTU.csv',h=TRUE,row.names = 1)
adonis(t(normalized_OTU)~soil_type+treatment+read_depth,data=meta_combine,permutations = 100)

```


