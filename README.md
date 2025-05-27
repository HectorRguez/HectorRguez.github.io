# DPO Dataset Annotation Interface

A web-based tool for annotating Direct Preference Optimization (DPO) datasets. This interface allows you to compare pairs of model responses and mark preferences for training preference-based models.

## Features

- **Fast annotation workflow**: Click a response to immediately save and advance
- **Auto-save functionality**: Progress is automatically saved to browser storage
- **Resume capability**: Continue from where you left off
- **Smart navigation**: Automatically skips already annotated items
- **Markdown rendering**: Properly displays formatted text, code blocks, and lists
- **Company name highlighting**: Automatically highlights advertiser names in responses
- **Progress tracking**: Visual progress bar showing completion status
- **Keyboard shortcuts**: Speed up annotation with hotkeys

## Getting Started

1. Save the HTML file to your computer
2. Open it in a web browser
3. Load your dataset using the "Load Dataset" button
4. Start annotating!

## Dataset Format

The tool expects a JSON file with an array of objects. Each object must have the following structure:

### Required Fields

```json
{
  "prompt": "The instruction/prompt given to the model",
  "response_1": "First model response to compare",
  "response_2": "Second model response to compare", 
  "preference": "",
  "metadata": {
    "question": "Original question asked",
    "original_answer": "Original response without advertisements",
    "response_1_ad_metadata": {
      "top_products": [
        {
          "name": "Product Name",
          "description": "Product description",
          "category": "Product category"
        }
      ]
    },
    "response_2_ad_metadata": {
      "top_products": [
        {
          "name": "Product Name", 
          "description": "Product description",
          "category": "Product category"
        }
      ]
    }
  }
}
```

### Field Descriptions

| Field | Type | Description | Required |
|-------|------|-------------|----------|
| `prompt` | string | The instruction/prompt given to the model | ✅ Yes |
| `response_1` | string | First response option for comparison | ✅ Yes |
| `response_2` | string | Second response option for comparison | ✅ Yes |
| `preference` | string | Annotation result (filled by the tool) | ✅ Yes |
| `metadata` | object | Additional information about the responses | ✅ Yes |
| `metadata.question` | string | Original question asked | ✅ Yes |
| `metadata.original_answer` | string | Original response without ads | ✅ Yes |
| `metadata.response_X_ad_metadata` | object | Advertisement metadata | ❌ Optional |
| `metadata.response_X_ad_metadata.top_products` | array | Product information for highlighting | ❌ Optional |

### Preference Values

The `preference` field will be automatically filled with one of these values:

- `"response_1"` - Response 1 was preferred
- `"response_2"` - Response 2 was preferred  
- `"both_good"` - Both responses are acceptable
- `"both_bad"` - Both responses are poor quality

### Example Dataset

```json
[
  {
    "prompt": "Explain what a .ckpt file is in machine learning",
    "response_1": "A .ckpt file stores model state...",
    "response_2": "A .ckpt file, used in ML, contains...",
    "preference": "",
    "metadata": {
      "question": "What is a .ckpt file for machine learning?",
      "original_answer": "A .ckpt file stores the current state...",
      "response_1_ad_metadata": {
        "top_products": [
          {
            "name": "TensorFlow",
            "description": "Open source ML platform",
            "category": "Software/Tools"
          }
        ]
      },
      "response_2_ad_metadata": {
        "top_products": [
          {
            "name": "PyTorch", 
            "description": "Deep learning framework",
            "category": "Software/Tools"
          }
        ]
      }
    }
  }
]
```

## Usage Instructions

### Loading Data
1. Click "Load Dataset" 
2. Select your JSON file
3. The tool will automatically resume from previous progress if available

### Annotation Controls

**Mouse Controls:**
- Click Response A or B to prefer that response
- Click "Both Good" if both responses are acceptable  
- Click "Both Bad" if both responses are poor quality

**Keyboard Shortcuts:**
- `1` - Select Response A
- `2` - Select Response B
- `G` - Mark both as good
- `B` - Mark both as bad
- `←/→` - Navigate between items

### Navigation Options
- **Forward**: Annotate from beginning to end
- **Backward**: Annotate from end to beginning  
- Use Previous/Next buttons for manual navigation

### Auto-Save Features
- Progress is saved automatically after each annotation
- Data is backed up to browser storage every 30 seconds
- Progress is preserved if you close the browser accidentally

### Downloading Results
- Click "Download Results" to save your annotated dataset
- File will be saved as `annotated_dpo_dataset.json`
- Original dataset remains unchanged

## Browser Compatibility

Works with all modern browsers including:
- Chrome/Chromium
- Firefox  
- Safari
- Edge

## Data Privacy

- All processing happens locally in your browser
- No data is sent to external servers
- Annotations are stored in browser's local storage
- Original dataset files remain on your computer

## Troubleshooting

**"Error parsing JSON file"**
- Ensure your file is valid JSON format
- Check that all required fields are present
- Verify proper array structure

**Progress not saving**
- Check if browser has local storage enabled
- Ensure sufficient storage space available
- Try refreshing the page and reloading dataset

**Company names not highlighting**
- Verify `ad_metadata.top_products` contains product names
- Check that product names are spelled correctly in responses
- Names in URLs will not be highlighted (by design)

## Technical Details

Built with vanilla HTML, CSS, and JavaScript. Uses:
- **Marked.js** for Markdown rendering
- **Local Storage API** for progress persistence  
- **File API** for dataset loading
- **Blob API** for downloading results

No external dependencies or server required.