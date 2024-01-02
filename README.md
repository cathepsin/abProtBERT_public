<body><h1>αβ-ProtBERT</h1>
<br>
<p>
Current version of the JLPrice lab ProtBERT implementation. Modified and in-testing to process batches of sequences.
<br><br>
A simple implementation of BERT for multiple-mask protein predictions for use with Google Colab.
This implementation uses the <a href=https://huggingface.co/Rostlab/prot_bert>prot_bert</a> pretrained model.<br>
<br>
Eventually a full python implementation will be made for usage on local or remote machines.
<br>
This implementation supports both fine-tuning and multiple-mask MLM predictions.<hr>
<br>
Refer to this README for running instructions and for an explanation of all various settings available.
<hr>
<h2>Configuration_Settings</h2>
<p>These settings relate to how the program will be run.</p>
<h3>use_custom_weights</h3>
<p>This setting specifies whether or not custom weights should be used. The following are allowed values for this setting:</p>
<ul>
    <li><strong>"No"</strong> : Use default weights</li>
    <li><strong>"From Computer"</strong> : Use a custom weights file from your computer</li>
    <li><strong>"From Google Drive or Colab"</strong> : Use a custom weights file from your Google Drive or your current Colab session</li>
</ul>
<h3>path_to_file_in_google</h3>
<p>This setting is only used if <code>use_custom_weights</code> is set to <code>"From Google Drive or Colab</code> and specifies where a weights file can be found.
The required file path can be most easily obtained by running the first cell of the main program titled <code>Setup, Imports, and Connect to Runtime</code> to mount your drive
 and then navigating the Colab file explorer to find your file, right-clicking it, and then selecting "Copy as path".</p>
<h3>fine_tune_model</h3>
<p>This setting specifies whether or not the model should be fine tuned before performing an MLM prediction session. Note that this can take several hours to complete. Acceptable values are (without quotation marks):</p>
<ul>
    <li><strong>true</strong> : Perform fine tuning session</li>
    <li><strong>false</strong> : Skip fine tuning session</li>
</ul>
<h3>MLM_prediction</h3>
<p>This setting specifies whether or not the model should perform an MLM prediction session. Acceptable values are (without quotation marks):</p>
<ul>
    <li><strong>true</strong> : Perform an MLM prediction session</li>
    <li><strong>false</strong> : Skip MLM prediction session</li>
</ul>
<hr>
<h2>Fine_Tune_Settings</h2>
<p>These settings are only used if <code>fine_tune_model</code> is set to <code>true</code></p>
<p>-------------------------------------------------------------------------------------------------------------</p>
<h3>FT_Settings</h3>
<p>-------------------------------------------------------------------------------------------------------------</p>
<h3>fine_tune_file_source</h3>
<p>This setting specifies the source where the fine tune file can be found. The fine tuning file should be a <code>.txt</code> file, organized
with one full protein/peptide sequence per line. <a href="https://github.com/cathepsin/protein_BERT_DATASETS">Example fine tuning files can be found here</a>.
Allowed values are the following:</p>
<ul>
    <li><strong>"From Computer"</strong> : Use a fine tune file from your computer</li>
    <li><strong>"From Google Drive or Colab"</strong> : Use a fine tune file from your Google Drive or your current Colab session</li>
</ul>

<h3>path_to_fine_tune_file</h3>
<p>This setting is only used if <code>fine_tune_file_source</code> is set to <code>"From Google Drive or Colab</code> and specifies where a fine tune file can be found.
The required file path can be most easily obtained by running the first cell of the main program titled <code>Setup, Imports, and Connect to Runtime</code> to mount your drive
 and then navigating the Colab file explorer to find your file, right-clicking it, and then selecting "Copy as path".</p>

<h3>percent_to_mask</h3>
<p>This setting specifies what percentage of tokens to mask during fine tuning. A suggested default from the
<a href="https://arxiv.org/abs/1810.04805">original BERT paper</a> is <strong>0.15</strong>. This value should be a real number
between 0 and 1, and requires a leading <code>0</code> before the decimal point.</p>

<h3>max_length</h3>
<p>This setting specifies the maximum length of a protein sequence to view during fine tuning. If a sequence is
encountered that exceeds the specified maximum length, the sequence will be truncated. This value should be an integer.</p>

<h3>batch_size</h3>
<p>This setting specifies how many groups your set of sequences provided in your fine tune file should be organized into. A more 
rigorous understanding of how batch sizes can alter fine tuning can be found elsewhere. In short, though, a higher batch size results in
faster and more course training, while smaller batch sizes result in the opposite.</p>

<h3>number_of_epochs</h3>
<p>This setting specifies how many times the fine tune file should be processed. Time required to complete fine tuning increases
<em>linearly</em> with the number of epochs. More epochs typically results in higher training accuracy, but can result in overfitting
a model if done in excess. Consult other sources for a more rigorous understanding of how to select a correct number of epochs to avoid
overfitting a model. A rather safe number (though admittedly naive to specific fine tune files) is around 100.</p>

<h3>learning_rate</h3>
<p>This setting controls how swiftly the model can be fine tuned. For a comprehensive, but somewhat esoteric, description of how learning rates
can alter fine tuning, <a href="https://pytorch.org/docs/stable/generated/torch.optim.Adam.html">conslut the optimizer documentation.</a> In short,
a higher learning rate causes the model to fine tune in more drastic and "grainy" steps, while a smaller value causes smaller and more "fine" steps.
A value around <strong>0.01</strong> is considered very large, and a value of <strong>0.00001</strong> is considered very small.</p>
<p>-------------------------------------------------------------------------------------------------------------</p>
<h3>Save_Settings</h3>
<p>-------------------------------------------------------------------------------------------------------------</p>
<h3>save_every_number_of_steps</h3>
<p>This setting specifies how often to save the progress of the fine tuning process. Since fine tuning can often take several hours,
this setting can allow for halting and starting-later fine tuning. Note that this setting refers to the number of <strong>steps</strong>,
not the number of <strong>epochs</strong> completed. Each epoch can have a variable number of steps determined by the batch size. This
value should be an integer. If <code>0</code> is used, then the state will only be saved at the end of the session.</p>

<h3>save_final_to_drive</h3>
<p>This setting specifies whether or not the final state should be saved to your Google drive. Note that a weights file is around 1.6 gigabytes.
The recommended way to save a weights file is to save it first to your Google Drive, download that file <em>from your Drive</em> to your
computer, and then delete it from your Drive to save space. Accepted values are:</p>
<ul>
    <li><strong>true</strong> : Save final state</li>
    <li><strong>false</strong> : Skip final state</li>
</ul>

<h3>download_final_state</h3>
<p>This setting specifies whether or not to directly download the final state to your computer. Note that a weights file is around
1.6 gigabytes. Though this feature is available, it is <em>not recommended</em> due to oddities in how Google Colab downloads files compared
to how it simply saves files to Google Drive.</p>
<hr>
<h2>MLM_Settings</h2>
</body>