# Troubleshooting Common Installation Issues

This section lists the most common problems users encounter during installation or setup of MONICA, along with solutions.  
It covers Python environment issues, dependency conflicts, Docker/Singularity problems, and configuration file errors.

---

# 1. Python / Environment Issues

### **Error: `ModuleNotFoundError: No module named 'numpy'`**
**Cause:** Dependencies not installed.  
**Fix:**

```
pip install -r requirements.txt
```
