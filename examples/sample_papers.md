# Sample Reference List

This is an example reference list for testing `/verify-refs`.

---

## Valid References

### GPT-4 Technical Report ★★★ 必引
```bibtex
@article{gpt4_2023,
  title={GPT-4 Technical Report},
  author={OpenAI},
  journal={arXiv:2303.08774},
  year={2023},
  url={https://arxiv.org/abs/2303.08774}
}
```
**Notes**: Well-formed entry with all required fields.

### Attention Is All You Need ★★★ 必引
```bibtex
@inproceedings{transformer2017,
  title={Attention Is All You Need},
  author={Vaswani, Ashish and Shazeer, Noam and Parmar, Niki and others},
  booktitle={NeurIPS},
  year={2017},
  url={https://arxiv.org/abs/1706.03762}
}
```
**Notes**: Classic paper with DOI available.

### BERT: Pre-training of Deep Bidirectional Transformers ★★ 选引
```bibtex
@inproceedings{bert2019,
  title={BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding},
  author={Devlin, Jacob and Chang, Ming-Wei and Lee, Kenton and Toutanova, Kristina},
  booktitle={NAACL},
  year={2019},
  url={https://aclanthology.org/N19-1423/}
}
```

---

## References with Issues (for testing)

### Missing Year (Red Flag)
```bibtex
@article{missing_year_example,
  title={This Entry Is Missing a Year Field},
  author={Test Author}
}
```
**Expected**: 🚨 Red flag - missing required field `year`.

### Missing URL (Yellow Flag)
```bibtex
@article{no_url_example,
  title={This Entry Has No URL or DOI},
  author={Another Author},
  year={2024}
}
```
**Expected**: ⚠ Yellow flag - no verifiable identifier.

### Invalid arXiv ID
```bibtex
@article{bad_arxiv,
  title={Entry with Invalid arXiv Format},
  journal={arXiv:invalid-format},
  year={2024}
}
```
**Expected**: 🚨 Red flag - invalid arXiv ID format.

---

## Self-Citation Example

### My Previous Work ★★★ 必引
```bibtex
@inproceedings{my_work2025,
  title={My Awesome Research Paper},
  author={Yu, Haorui and others},
  booktitle={EMNLP},
  year={2025},
  note={自引工作}
}
```
**Notes**: Self-citation with author variant check.
