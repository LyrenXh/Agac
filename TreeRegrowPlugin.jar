package your.plugin;

import org.bukkit.*;
import org.bukkit.block.Block;
import org.bukkit.event.*;
import org.bukkit.event.block.BlockBreakEvent;
import org.bukkit.plugin.java.JavaPlugin;
import org.bukkit.scheduler.BukkitRunnable;

import java.util.*;

public class TreeRegrowPlugin extends JavaPlugin implements Listener {
    private final Set<Material> logTypes = EnumSet.of(
        Material.OAK_LOG, Material.BIRCH_LOG, Material.SPRUCE_LOG,
        Material.JUNGLE_LOG, Material.DARK_OAK_LOG, Material.ACACIA_LOG,
        Material.CHERRY_LOG
    );

    @Override
    public void onEnable() {
        Bukkit.getPluginManager().registerEvents(this, this);
        getLogger().info("TreeRegrowPlugin etkin!");
    }

    @EventHandler
    public void onBlockBreak(BlockBreakEvent event) {
        Block block = event.getBlock();
        if (!logTypes.contains(block.getType())) return;
        if (event.getPlayer().getGameMode() != GameMode.SURVIVAL) return;

        List<Block> treeBlocks = new ArrayList<>();
        findTree(block, treeBlocks, new HashSet<>());
        treeBlocks.forEach(Block::breakNaturally);

        Location base = block.getLocation();
        Material sapling = getSaplingForLog(block.getType());
        if (sapling != null) {
            new BukkitRunnable() {
                @Override
                public void run() {
                    base.getBlock().setType(sapling);
                    base.getWorld().spawnParticle(Particle.VILLAGER_HAPPY, base.clone().add(0.5,0.5,0.5),10,0.3,0.3,0.3);
                }
            }.runTaskLater(TreeRegrowPlugin.this, 20*10);
        }
    }

    private void findTree(Block block, List<Block> found, Set<Block> visited) {
        if (!logTypes.contains(block.getType()) || visited.contains(block)) return;
        visited.add(block);
        found.add(block);
        for (BlockFace face : BlockFace.values()) {
            findTree(block.getRelative(face), found, visited);
        }
    }

    private Material getSaplingForLog(Material log) {
        return switch (log) {
            case OAK_LOG -> Material.OAK_SAPLING;
            case BIRCH_LOG -> Material.BIRCH_SAPLING;
            case SPRUCE_LOG -> Material.SPRUCE_SAPLING;
            case JUNGLE_LOG -> Material.JUNGLE_SAPLING;
            case DARK_OAK_LOG -> Material.DARK_OAK_SAPLING;
            case ACACIA_LOG -> Material.ACACIA_SAPLING;
            case CHERRY_LOG -> Material.CHERRY_SAPLING;
            default -> null;
        };
    }
}
