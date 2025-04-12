πEngine Technical Documentation
Nightly Build 11.04.2025-21:30 | GPL-3.0
Commit: 3f14159a265 (Vulkan-render branch)

Core Architecture
1. Platform Layer
python
Copy
# platform.py
class Platform:
    @staticmethod 
    def init():
        if sys.platform == 'linux':
            from .linux import VulkanContext  # Native Linux implementation
        elif sys.platform == 'win32':
            from .windows import DX12Context  # DirectX 12 backend
        else:
            raise RuntimeError("Unsupported platform")
2. Render Pipeline
python
Copy
# renderer/vulkan.py
class VulkanRenderer:
    def __init__(self):
        self._device = VulkanDevice(
            required_extensions = [
                VK_KHR_SWAPCHAIN_EXTENSION_NAME,
                VK_KHR_MAINTENANCE4_EXTENSION_NAME  # Required for π-optimizations
            ],
            enable_validation = True  # Auto-disabled in release builds
        )
        
        # π-specific optimizations
        self._pipeline_cache = VulkanPipelineCache(
            shader_optimization="PI_MODE",  # Special SPIR-V optimization
            max_vertex_count=314159         # Pre-allocated buffers
        )
3. Mathematics Library
python
Copy
# math/pi_math.py
PI_PRECISION = 3.14159265358979323846  # 20 decimal places

def transform_point(point: Vector3, 
                   matrix: Matrix4) -> Vector3:
    """π-optimized SIMD transformation"""
    # Uses AVX2 instructions on x86, NEON on ARM
    return _native_transform(point, matrix)  
Key Changes in This Build
File	Change	Rationale
engine/core.py	Removed OpenGL fallback	Vulkan/DX12 are now mandatory
input/linux.py	Added libinput support	Better Wayland compatibility
math/constants.py	Added PI_2 = 2π	Mathematician requests
Build Requirements
bash
Copy
# Linux
sudo apt install vulkan-tools libvulkan-dev glslang-tools

# Windows
choco install vulkan-sdk --version=1.3.250
Runtime API Changes
python
Copy
# Old (pre-11.04)
pe.init(use_opengl=True)  

# New
pe.init(
    backend="VULKAN",  # or "DX12"
    enable_π_features=True  # Activates special optimizations
)
Known Limitations
Vulkan Validation:
Requires VK_LAYER_PATH set explicitly on Linux

bash
Copy
export VK_LAYER_PATH=/usr/share/vulkan/explicit_layer.d
Multithreading:
πWorker threads limited to 3.14159× core count (safety threshold)

Debugging Tools
python
Copy
# Enable advanced diagnostics
pe.debug.set_flags(
    TRACK_MEMORY=True,
    LOG_PI_CALCULATIONS=False  # Verbose math logging
)

# Generate crash report
pe.debug.capture_state("crash_report.πdbg")
This documentation reflects the exact state of nightly-11.04.2025 branch.
For runtime checks, use:

python
Copy
if pe.version() != "11.04.2025-21:30":  
    pe.log.warning("Documentation mismatch!")  
